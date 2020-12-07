### Chapter 2 : Principles in Refactoring

Definition :

> Usage as noun meaning : a change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior.

> Usage as verb meaning : to restructure software by applying a series of refactorings without changing its observable behavior.

**Lessons learnt** :

- Refactoring is not just cleaning up the code. It help cleaning up of the code as well as structuring/organising the code in a better way, so that there is a clean pattern/flow established. 

- Refactoring should have zero impact to end user.

- Performance optimisation can be one of the exceptions where refactoring might not help. Sometimes we need to write complex piece of code for improving execution time of code.

- Learn to wear two hats: 

  - Try to divide time/effort between adding fn / features and refactoring.
  - Keep both the things mutually exclusive.
    - If you wore addition of fn hat, don't refactor code. Just add new fn and write tests for new functionality
    - If you wore refactoring hat, don't add new fn, refactor code and run existing tests to ensure that the existing functionality is intact. Add more tests only if there is one missing for existing flow.

  #### Why refactor ?

  - It improves design of software
    - Loss of the design of code has a cumulative effect. It becomes harder to see the code, visualise design , preserve it ultimately leading to decaying of software.
    - Refactoring remove bits and pieces that aren't useful. More like tidying up of the code.
    - Existing culprits like code duplication, short term feature implementations etc bloat code and makes it harder to add new features and take more time / complexity to be implemented.
  - It make software easier to understand
    - Often there will be `n` number of people working on same codebase. So refactoring also help fellow dev to understand the code.
    - You should look to minimise the gap between what you want your code to do and what you tell it to do
    - Always write new code thinking how easy you can make it for some new dev to understand and extend code functionality.
    - Put all the details in code so that you can flush of your memory without having to remember context further.
  - It helps you find bugs
    - When you refactor you find and understand code flows that you might have otherwise overlooked. Also there are high chances to find potential bug since you understand the flow much better now.
  - It helps you program faster
    - This was our ultimate goal for doing refactoring. 
    - A well designed code makes it very easier to add more features quickly and unlocks rapid development.
    - Poorly designed code can help you add fn/features in short term, but becomes very tricky and slows you down in long run, since it becomes to complex to understand.

#### When should you refactor ?

Definetely allocation special time for doing refactoring won't help. Because then you are trying to find something to fix, rather than found something wrong and are trying to fix it.

Refactoring should be done in small bursts. You don't decide to refactor, you refactor because you want to do something else, and refactoring helps you do that other thing.

Few things that help you decide when to refactor : 

- Rule of 3 
  - The first time you do something, you just do it. The second time you do something similar, you wince at the duplication, but you do the duplicate thing anyway. The third time you do something similar, you refactor.
- Refactor When You Add Function
  - When adding new feature , refactor the existing code first if required. It will help you undestand code you need to modify
  - If you see a bad design with existing code, refactor it before adding new feature that may complicate the code even more.
- When fixing a bug
  - If there is a bug reported in code, that directly implies that there is something wrong with the design and/or code. Fix it and make it clear enough
- During code reviews
  - If you are doing a code review, pair with the person raising the PR to refactor a bad piece of code
  - With larger design reviews it is often better to obtain several opinions in a larger group. Showing code often is not the best device for this
  - Seek code review from team, as your code will always look good to you.
  - Do more of pair programming while adding fn

#### How to convince manager ?

If your manager is Engg. manager it might be easy to convince him/her as they might be aware of the impact the refactoring exercise can bring in long run.

However if the case is otherwise try to convince your manager with the help of some books , blogs, lectures which explain the advantage of it. 

If still there is no luck. Author says, Don't tell!

Take some time out during feature implementation, bug fixes to do the exercise silently, since you understand the importance of it.

#### Problems with refactoring 

> Note : Apart from the things listed below, there can be some unknown effects of refactoring too.

- Databases
  - Most of the applications are tightly coupled too the db schema supporting them. So it becomes difficult to change db or code. 
  - Even if a change could be done , due to copuling you need to migrate the data related to the change.
    - This again is problem, since most of the applications do not work with automatic data migration techniques.
    - Even if they do(object databases), migration process takes some time to happen.
- Interfaces 
  - Changing objects is easy to do as we know the places where it is being used and can be refactored.
  - But change in interface might have issues as you are never 100% certain where and all the affected code is being used
  - Ex: Renaming a function of interface.
- Few design changes are difficult to refactor
  - When there is a bad design in the cental part of codebase, it becomes hard to change, as there can be unknown parts of code that might get affected

#### When shouldn't you refactor ?

- When existing code is such a mess, that refactoring won't help. Go for a rewrite of it.
- When code has so many bugs that it is hard to stablise.
- (Not recommened) When you are close to deadline and don't have time for refactoring. This code becomes debt then, hence should be refactored post release or if it bearable debt then it's okay. 



#### Refactoring and Design

> Refactoring complements design a lot

- It is good to think and design a good system before implementing it(code)
- Refactoring is an alternative of upfront good design. It is supposed to be done, when there is a bad design in already implemented code.
- Extreme programming(XP) advocates otherwise. You implement a working solution and then refactor it. But they do some level of upfront designing and decision making too.
- A lot of refactoring at later point becomes expensive.
- People trying to avoid refactoring later try to design flexible systems for future. This sometimes becomes hard to maintain and complex designs. Sometimes this flexibilty is not needed at all.
- Refactoring leads to simple designs without sacrificing flexibility.

#### Refactoring and performance

Problem with refactoring is that to make the code/design simpler, you'll often make changes that cause program to run slowly. Softwares are rejected for being too slow.

There are 3 approaches to writing fast software :

- Most serious is time budgeting
  - Decompose the design, give each component a hard time and resources budget. This proccess focusses on hard performance times only.
- Constant attention approach
  - Programmers working on codebase keep a constant watch on hard performance times of software while working on it. This might not really help in long run as with times software becomes hard to work on as there are lot of performance optimisation code here and there. And these piece of code is mostly complex.
- Build a well designed/structured software first and then refactor.
  - This helps in retaining good design practices as well as gaining performance. But this might take some additional time to complete.
  - This helps you add more fn quickly in long run as you have well structured code.

#### Where did refactoring came from ?

Two of the first people to recognize the importance of refactoring were Ward Cunningham and Kent Beck, who worked with Smalltalk from the 1980s onward.

Smalltalk has a very short compile-link-execute cycle, which makes it easy to change things quickly. It is also object oriented and thus provides powerful tools for minimizing the impact of change behind well-defined interfaces. Ward and Kent worked hard at developing a software development process geared to working with this kind of environment. (Kent currently refers to this style as Extreme Programming [Beck, XP].)
