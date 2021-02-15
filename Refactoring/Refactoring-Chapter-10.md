# Refactoring: improving the design of existing code Notes

# Chapter 10 : Making Method calls simpler

There are several ways in which it can be achieved, however there are few things to remember :

- Method name should explain the functionality of it
- In OOPs dont pass too many arguments to methods
- In concurrent language reducing too many params might not be possible as the contract defines immutable values whereas objects can be mutable
- Always break single method doing both state change and querying data into subsequent methods to seperate out logic

#### Types of refactoring that can be done :

1. **Rename method :**

   If a method name does not relate to its purpose , change the name accordingly

   ```java
   dispVldAmt()
   ```

   To

   ```java
   displayValidAmount()
   ```

   **Motivation :**

   - Good way to find a name of the method is to relate with what comment will you give to method
   - Your code is a for humans more than for machines and hence names should be meaningful
   - Good naming requires practice 

2. **Add parameter**

   When a method needs more information from its caller, pass new parameter to it but instead see if you can pass an object if number of parameters are more than one.

   ```java
   getInvoice()
   ```

   To

   ```java
   getInvoice(Date)
   ```

   **Motivation**

   - Long parameter lists smell bad because they are hard to remember and often involve data clumps

3. **Remove Parameter**

   When a method doesn't require a parameter , remove it then and there.

   ```
   getInvoice(Date)
   ```

   To

   ```
   getInvoice()
   ```

    **Motivation**	

   - Parameter list should alway inform about the details needed by the method
   - Method caller has to worry about passing the information increasing work for caller
   - In most cases when caller knows the parameter is not being used , it'll pass null, which is again bad

4. **Separate Query from Modifier**

A method should not do both , change state of object as well as queries the state. In such cases create two methods, one for query another for modification

```
getTotalOutstandingAmountAndSetDues()
```

To

```
getTotalOutstandingAmount()
setDues()
```

**Motivation**

- There is less to worry about when you have a method to query without having side effects out of it(state change)
- It can be called anywhere, any number of times
- Having two different methods also removes worrying about order of execution of methods and any concurrency issues.

5. **Parameterize Method**

When several methods do similar stuff but return different values depending on some hardcoded values, consider moving it to same function passing extra argument.

```
tenPercentRaise()
fivePercentRaise()
```

To

```
raise(percentage)
```

**Motivation**

- Reduces code footprints
- Removes duplicate code
- Adding other variations becomes easy without having to write new methods

6. **Replace Parameter with Explicit Methods**

You have a method that runs different code depending on the values of an enumerated parameter. Create a separate method for each value of the parameter.

```java
void setValue (String name, int value) {  
  if (name.equals("height"))         
    _height = value;      
  if (name.equals("width"))    
    _width = value;     
  Assert.shouldNeverReachHere();   
}
```

To

```java
void setHeight(int arg) {     
  _height = arg;  
} 
void setWidth (int arg) {
  _width = arg; 
}
```

**Motivation**

- You not only avoid the conditional behavior but also gain compile time checking. 
- Interface becomes clear and more readable

7. **Preserve Whole Object**

   When you need to extract values from a object and send it to a method as arguments, consider sending whole object itself

   ```java
   int low = daysTempRange().getLow();
   int high = daysTempRange().getHigh();    
   withinPlan = plan.withinRange(low, high);
   ```

   To

   ```java
   thinPlan = plan.withinRange(daysTempRange());
   ```

   **Motivation**

   -  if the called object needs new data values later, you have to find and change all the calls to this method
   - It is more readable
   - Long parameter lists can be hard to work with because both caller and callee have to remember which values were there.
   - But when the calling method needs only one value from object, it might be better to pass just single value from object.

8. **Replace Parameter with Method**

   An object invokes a method, then passes the result as a parameter for a method. The receiver can also invoke this method. Remove the parameter and let the receiver invoke the method.

   ```java
   int basePrice = _quantity * _itemPrice; 
   discountLevel = getDiscountLevel();    
   double finalPrice = discountedPrice (basePrice, discountLevel);
   ```

   To

   ```java
   int basePrice = _quantity * _itemPrice;  
   double finalPrice = discountedPrice (basePrice)
   ```

   **Motivation**

   - If a method can get a value that is passed in as parameter by another means, it should.
   - You can't remove the parameter if the calculation relies on a parameter of the calling method, because that parameter may change with each call (unless, of course, that parameter can be replaced with a method)

9. **Introduce Parameter Object**

You have a group of parameters that naturally go together, replace them with object

```java
amountInvoicedOn(start_date,end_date)
amountRecievedOn(start_date,end_date)
amountOverdueOn(start_date,end_date)
```

To

```java
amountInvoicedOn(DateRange)
amountRecievedOn(DateRange)
amountOverdueOn(DateRange)
```

**Motivation**

- Often you see a particular group of parameters that tend to be passed together. Several methods may use this group, either on one class or in several classes. Such a group of classes is a data clump and can be replaced with an object that carries all of this data. 
- You get a deeper benefit, however, because once you have clumped together the parameters, you soon see behavior that you can also move into the new class. 

10. **Remove Setting Method**

A field should be set at creation time and never altered. Remove any setting method for that field.

**Motivation**

-  If you don't want that field to change once the object is created, then don't provide a setting method (and make the field final).
- That way your intention is clear and you often remove the very possibility that the field will change.

11. **Hide Method**

    When a method is not used by any other class, either make it private or remove it.

    **Motivation**

    - A particularly common case is hiding getting and setting methods as you work up a richer interface that provides more behavior.

12. **Replace Constructor with Factory Method**

    You want to do more than simple construction when you create an object, replace the constructor with a factory method.

```java
Employee (int type) {    
_type = type; 
}
```

To

```java
static Employee create(int type) {     
	System.out.println("Creating constructor");
	return new Employee(type); 
}
```

**Motivation**

- The most obvious motivation for Replace Constructor with Factory Method comes with replacing a type code with subclassing.

13. **Encapsulate Downcast**

A method returns an object that needs to be downcasted by its callers, move the downcast to within the method.

```java
Object lastReading() {   
	return readings.lastElement();
}
```

To

```java
Reading lastReading() {   
	return (Reading) readings.lastElement();
}
```

**Motivation**

- Downcasting may be a necessary evil, but you should do it as little as possible
- Often you find this situation with methods that return an iterator or collection. Look instead to see what people are using the iterator for and provide the method for that.

14. **Replace Error Code with Exception**

    A method returns a special code to indicate an error, throw an exception instead.

```java
int withdraw(int amount) {    
	if (amount > _balance)
  return -1;
  else {
  _balance -= amount;
  return 0; 
  }
}
```

To

```java
void withdraw(int amount) throws BalanceException {    
	if (amount > _balance) throw new BalanceException();
  _balance -= amount;
}
```

**Motivation**

- If the cost of a program crash is small and the user is tolerant, stopping the program is fine.
-  Unix and C-based systems traditionally use a return code to signal success or failure of a routine.
- Exceptions are better because they clearly separate normal processing from error processing. 
- This makes programs easier to understand, and as I hope you now believe, understandability is next to godliness.

15. **Replace Exception with Test**

You are throwing a checked exception on a condition the caller could have checked first, change the caller to make the test first.

```java
double getValueForPeriod (int periodNumber) {     
  try {     
  	return _values[periodNumber];  
  } 
  catch (ArrayIndexOutOfBoundsException e) {      
  	return 0;     
  }
}
```

To

```java
double getValueForPeriod (int periodNumber) {  
  if (periodNumber >= _values.length) return 0;   
  return _values[periodNumber]; 
}
```

**Motivation**

- If you can reasonably expect the caller to check the condition before calling the operation, you should provide a test, and the caller should use it.
