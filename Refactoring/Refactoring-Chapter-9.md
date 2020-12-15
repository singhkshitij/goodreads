### Chapter 9 : Simplifying conditional expressions

This basically means to break conditional experissions into small pieces. This helps seperate switching logic from actual conditional logic (in other words seperated what should happen from when it should happen).

#### Types of conditional refactoring :

1. **Decompose Conditional** : 

   If you have a complicated conditions and domain logic, seperate them and move to new fns/methods.

   ```java
   if (date.before (SUMMER_START) || date.after(SUMMER_END))    
     charge = quantity * _winterRate + _winterServiceCharge;    
   else charge = quantity * _summerRate;
   ```

   To 

   ```java
   if (notSummer(date))   
     charge = winterCharge(quantity);  
   else charge = summerCharge (quantity);
   ```

   **Motivation :**

   - Reduces method length
   - Easier to read
   - Communicates the purpose/intention clearly
   - Explains reason for branching code

   **Mechanics** : 

   - Extract the condition into its own method.

   - Extract the then part and the else part into their own methods.

2. **Consolidate Conditional Expression**

   If there are sequence of conditions with same output, combine them. If required extract it into seperate method since too many conditions can harm readability.

   ```java
    double disabilityAmount() {   
      if (_seniority < 2) return 0;
      if (_monthsDisabled > 12) return 0;
      if (_isPartTime) return 0;
      // compute the disability amount
   ```

   To

   ```java
   ouble disabilityAmount() {   
     if (isNotEligableForDisability()) return 0;  //isNotEligableForDisability has check for all conds combined
     // compute the disability amount
   ```

   **Motivation :**

   - Makes it clear that just on check with several Or/And is required to reach an output.
   - Most of the times it enables you to *Extract Method* which helps increase readability. It replaces a statement of what you are doing with why you are doing it

   > However, if code wants to really represent these checks as independent conditions, dont refactor.

   **Mechanics :**

   - Check that none of the conditionals has side effects. 
   - Replace the string of conditionals with a single conditional statement using logical operators.
   - Compile and test.
   - Consider using Extract Method on the condition

   > If there are just two conditions and your code communicates the purpose, try using conditional statements. Ex
   >
   > ```java
   > if (onVacation() && lengthOfService() > 10) return 1; 
   > 	else return 0.5;
   > 
   > becomes
   >   return (onVacation() && lengthOfService() > 10) ? 1 : 0.5;
   > ```

   3. **Consolidate Duplicate Conditional Fragments**

   When same fn call or same code is present in all branches of a conditional code, make it common and write once.

   ```java
         if (isSpecialDeal()) {  
           total = price * 0.95;      
           send();    
         }else {
           total = price * 0.98;send();
         }
   ```

   To 

   ```java
         if (isSpecialDeal())     
           total = price * 0.95;   
         else         
           total = price * 0.98;  
         send();
   ```

   **Motivation :**

   This makes clearer what varies and what stays the same.

   **Mechanics :**

   - Identify code that is executed the same way regardless of the condition.
   - If the common code is at the beginning, move it to before the conditional.
   - If the common code is at the end, move it to after the conditional.
   - If the common code is in the middle, look to see whether the code before or after it changes anything. If it does, you can move the common code forward or backward to the ends. You can then move it as described for code at the end or the beginning.
   - If there is more than a single statement, you should *extract that code into a method*.

> The same situation can apply to exceptions. If code is repeated after an exception-causing statement in the try block and all the catch blocks, I can move it to the final block.

4. **Remove Control Flag**

When there is a variable acting a flag , replace it with either return or break statement. Few languages have break and continue to server purpose rest will have break.

**Motivation **:

- The real purpose of the conditional becomes so much more clear.
- Such control flags are more trouble than they are worth. They harm readability, increase complexity of code.

**Mechanics :**

- For break and continue
  - Find the value of the control flag that gets you out of the logic statement.
  - Replace assignments of the break-out value with a break or continue statement.
  - Compile and test after each replacement

```java
   void checkSecurity(String[] people) {  
     boolean found = false;  
     for (int i = 0; i < people.length; i++) {  
       if (! found) {       
         if (people[i].equals ("Don")){    
           sendAlert();     
           found = true;     
         }     
         if (people[i].equals ("John")){    
           sendAlert();        
           found = true;        
         }          
       }       
     }   
   }
```

To

```java
void checkSecurity(String[] people) {     
  for (int i = 0; i < people.length; i++) {  
    if (people[i].equals ("Don")){     
      sendAlert();       
      break;    
    }         
    if (people[i].equals ("John")){    
      sendAlert();          
      break;     
    }
  }
}
```

- For return statements
  - Extract the logic into a method.
  - Find the value of the control flag that gets you out of the logic statement.
  - Replace assignments of the break-out value with a return.
  - Compile and test after each replacement

Prefer return whenever possible. It make code more clear when the execution has to stop.

> Sometimes the control flag might also contain the result statement, so return the value from method rather than just using empty return statement

```java
void checkSecurity(String[] people) {   
  String found = "";
  for (int i = 0; i < people.length; i++) { 
    if (found.equals("")) {            
      if (people[i].equals ("Don")){     
        sendAlert();            
        found = "Don";       
      }
      if (people[i].equals ("John")){     
        sendAlert();  
        found = "John"; 
      }
    }
  }
  someLaterCode(found);   
}
```

To 

```java
   String foundMiscreant(String[] people){  
     for (int i = 0; i < people.length; i++) {  
       if (people[i].equals ("Don")){ 
         sendAlert();            
         return "Don";        
       }         
       if (people[i].equals ("John")){    
         sendAlert();           
         return "John";   
       }
     }
     return "";
   }
```

5. **Replace Nested Conditional with Guard Clauses**

When there is a method with nested conditional cases which is not very clear, use guard clauses for all cases. It enabled multiple exit point in the code, whenever a condition is met

```java
  double getPayAmount() {   
    double result;  
    if (_isDead) result = deadAmount();  
    else {    
      if (_isSeparated) result = separatedAmount();    
      else {     
        if (_isRetired) result = retiredAmount();    
        else result = normalPayAmount();   
      };
    }
    return result;  
  };
```

To 

```java
 double getPayAmount() {   
   if (_isDead) return deadAmount();   
   if (_isSeparated) return separatedAmount(); 
   if (_isRetired) return retiredAmount();
	 return normalPayAmount();  
 };
```

**Motivation :**

- These kinds of conditionals have different intentions, and these intentions should come through in the code.
-  Instead the guard clause says, "This is rare, and if it happens, do something and get out."

**Mechanics :**

- For each check put in the guard clause.
- Compile and test after each check is replaced with a guard clause.

6. **Replace conditional with polymorphism**

This is applicable when you have a conditional statement that choses different behaviour based on type of object. You should move each leg of the conditional to an overriding method in a subclass, making the original method abstract.

>  The essence of polymorphism is that instead of asking an object what type it is and then invoking some behavior based on the answer, you just invoke the behavior. The object, depending on its type, does the right thing.

**Motivation :**

- The biggest gain occurs when this same set of conditions appears in many places in the program.
- If you want to add a new type, you have to find and update all the conditionals.
-  Clients of the class don't need to know about the subclasses, which reduces the dependencies in your system.

**Mechanics :**

- Pick one of the subclasses. Create a subclass method that overrides the conditional statement method. Copy the body of that leg of the conditional statement into the subclass method and adjust it to fit.
- Compile and test.
- Remove the copied leg of the conditional statement.
- Compile and test.
- Repeat with each leg of the conditional statement until all legs are turned into subclass methods.
- Make the superclass method abstract.

```java
class Employee...  
  int payAmount() {   
  switch (getType()) {   
    case EmployeeType.ENGINEER:  
      return _monthlySalary;   
    case EmployeeType.SALESMAN:  
      return _monthlySalary + _commission;    
    case EmployeeType.MANAGER:      
      return _monthlySalary + _bonus;   
    default:             
      throw new RuntimeException("Incorrect Employee");   
  }
}
```

To

```java
class EmployeeType...  
	private EmployeeType _type;

  int getType() {     
	  return _type.getTypeCode();   
	}    

  int payAmount(Employee emp) {  
  switch (getTypeCode()) {        
    case ENGINEER:
      throw new RuntimeException ("Should be being overridden");       
    case SALESMAN:           
      return emp.getMonthlySalary() + emp.getCommission();  
    case MANAGER:              
      return emp.getMonthlySalary() + emp.getBonus(); 
    default:
      throw new RuntimeException("Incorrect Employee");        
  }    
}

carry on until all the legs are removed...
```

Where EmployeeType class is

```java
 abstract class EmployeeType...  
   abstract int getTypeCode();

```

Subclass Engineer

```java
class Engineer extends EmployeeType...    
  int getTypeCode() {       
	  return Employee.ENGINEER;    
	}
	int payAmount(Employee emp) {   
    return emp.getMonthlySalary();    
  }
}
```



8. **Introduce Assertion**

   When a part of code makes certain assumptions, assert the code before executing the logic so that assumption is always met.

   ```java
    double getExpenseLimit() {       
      // should have either expense limit or a primary project  
      return (_expenseLimit != NULL_EXPENSE) ?           
        _expenseLimit:          
      _primaryProject.getMemberExpenseLimit(); 
    }
   ```

   To

   ```java
   double getExpenseLimit() {    
     Assert.isTrue(_expenseLimit != NULL_EXPENSE || _primaryProject != null);  
     return (_expenseLimit != NULL_EXPENSE) ?  
       _expenseLimit:          
     _primaryProject.getMemberExpenseLimit(); 
   }
   ```

   **Motivation :**

   - Failure of an assertion indicates programmer error. 
   - Clearly indicates the assumption code is making
   - Better than putting comment to explain same intention
   - Assertions act as communication and debugging aids.
   - In debugging, assertions can help catch bugs closer to their origin.

   

   **Mechanics :**

   - When you see that a condition is assumed to be true, add an assertion to state it
   - Don't use assertions to check everything that you think is true for a section of code. 
   - If the code works without the assertion, the assertion is confusing rather than helpful and may hinder modification in the future, remove it.

   > Since these are not helpful in prod, have them underside kind of flag so that it can be disabled based on env and compiler reads that as a dead code, not taking it to prod
   >
   > ```java
   > Assert.isTrue(**Assert.ON** &&     
   >               (_expenseLimit != NULL_EXPENSE || _primaryProject != null))
   > ```
   >
   > 
