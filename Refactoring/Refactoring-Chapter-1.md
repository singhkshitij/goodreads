# Refactoring: improving the design of existing code Notes

The book by M. Fowler talks about the best practices to refactor a peice of code alongwith explanation on how to make robust softwares with good design principles.



## What is Refactoring ?

It refers to any change made to code pieces of a software in order to improve software design without disturbing the end output or functionalities. It makes the software code more readable and understandable.

Refactoring is mostly done in a series of it to improve overall software structure.

Refactoring is opposite of ideal software dev principles :

- Take badly designed, chaotic code and rework it to a well designed one.
- Take smaller steps
- Keep changing designs as you refactor



### Chapter 1 : First Example

Example given : Movie rental store where users are charged based on diff parameters such as new movie, children movie or a regular movie.

Problem extended to print the statement in an HTML page. Suggested method Copy paste same fn. But causes problem if something is to be fixed in logic as there is code duplicacy.

**Lessons learnt** :

- If there is a complicated piece of code where a fix has to be done, simplify the existing logic first by refactoring then make logic changes. It will help in long run.
- Do not write lengthy fn/methods.

**How to do refactoring** :

- Ensure that you have tests well written to cover all the scenarios related to the changes first. 
- Tests should be self-checking(Doesn't just show failure but also what has failed and reason)
- Make small changes in batches so that catching bugs in case of test failures are easy to find
- Rename(if req) to make variable names more meaningful. We should aim at writing code for humans too.
- Code should easily communicate its purpose.
- Remove temp variables if unnecessary and are not used at multiple places. Use query methods instead (ex- getPrice())
- Use ploymorphism whenever the resposibilites are not properly distributed within classes. (Ex- Method more suitable in class X is currently present in Y but accesses class X's variables for BL)
- Use inheritance whenever required. (Ex- Parent class Cars can get price of different type of cars from subclasses and need not to compute within itself). This enables extensibility. (Ex- more car types can be added easily without touching parent class) 
- Rhythm of refactoring: test, small change, test, small change, test, small change
