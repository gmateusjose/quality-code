# Clean code

# Meaningful Names

About everything in programming relates to some kind of names: variables, classes, functions, attributes, methods... Names can pass much information about code, even more than comments. When naming something think very carefully in concepts like:

- Why is exists?
- What it does?
- How is used?

Here we have some rules of thumb when naming things in your code:

- **Add meaningful names and contexts:** This helps you to clarify your code, a variable called `purchases` is much more informative than a variable called `p`. avoid non-meaningful names like `Manager`, `Processor`, `Data` or `Info`.
- **One word per concept:** When expressing your names is easier to express your ideas using one word per concept, like `nonPremiumPurchases` and `premiumPurchases`. Avoid using names like `fetch`, `retrieve` and `get` together and in the same code base, this could lead to confusion.
- **Don't encode:** You don't need to write the type of your variable in their name, because this pollutes the code and is very not meaningful.
- **Don't use small names variation:** It could also lead to confusion. Things that are hard to distinguish makes your code harder to read.
- **Avoid false clues:** Definitively putting false clues in your code will lead to an extremous amount of effort to grasp what is happening in your code.

# Functions

In a clean code functions should be small, with descriptive names (like any other thing in your code), do only one thing (follows the Single responsibility principle) and must not have any behaviour repeated over and over again.

When writing functions make only one abstraction lever per function, do not mix in a function abstract behaviours and implementations and rules of business. This violates the SRP (Single responsibility principle). Always **NEVER USE FLAGS ARGUMENTS** (passing a bool value to a function), this indicates that your function does more than one thing at once. And last, the ideal scenario is that a function shouldn't use any arguments, the complexity of the function increases exponentially with the number of arguments used, so never use more than 3 arguments per function.

Here we go with some observations: Write functions just to handle errors of other functions, encoding arguments into function's names its good to quickly grasp what that function needs. And prefer using exceptions instead of returning error codes. 

# Comments

When dealing with comments is better to explain yourself in code, rather than spend your time writing comments that explain the mess you've made. In a plain explanation any comments that forces you to look in another place for grasp the meaning of it has failed to communicate. And the first premisse is **only the code can truly tell you what it does.**

Although in a codebase we could also have good comments that could provide explanations of intent, warning of consequences and some amplification (explain more than just the code can). We also could have legal comments (copyright or licensing). Todo comments - reminders of what we must do. And documentation in Public APIs.

# Formatting

Code formatting is about communication, so the first rule of thumb is: If the developer works on a team, follow the team rules. Here we have some patterns that could make your code formatting more consistent:

**Horizontal formatting:** 

- You should never scroll to the right, but take a upper limit of 120.
- Prefer to indent one scope (many indentation levels can express that a function has more than one level of abstraction).
- Use a blank space to associate or disassociate things strongly related, for example: `compute(firstTerm=5, secondTerm=8)` (blank space after comma)

**Vertical formatting:**

- Small files are easy to read, Typically 200 lines is good. Never keep files bigger than 500 lines.
- Each group of lines represents a complete thought and these should be separated with a blank line. And lines of code and concepts that are tightly related should appear vertically close to each other.
- Use the newspaper metaphor: The name of the file should be simple and explanatory; The topmost parts should provide the high-levels concepts and algorithms, and the details increase as we move downwards.

# Objects and Data structures

The first thing that we need to understand in order to Grasp the best practices about objects and data structures, is to definitely understand what is an Object abstraction, and a Data structure abstraction. Both structures can be represented using OO.

An Object is a way of expressing data where we usually want to hide inner variables, we just want to expose functions. A good way of thinking about is: "We should tell a good object to do something".  Generally thinking a good object hides data, and expose functions.

Meanwhile, a Data structure is another way to express data inside a system, in a data structure, we only care about the variables inside it, with no function defined. That means, we expose data and have no functions. In someway a Data structure is the complete opposite of an Object, we could think about data structures, like: "We should only ask a data structure about its internals". Commonly a data structure is a class with public variables and without any functions, which makes it perfect to deal with database abstractions.

A very common type of data structure is a Active Record, that are data structures with public variables, but with some methods like `save` and `find`. Active Records are direct translations from database tables.

When dealing with objects and data structures, we should think about some things:

1. Things that are hard for objects are easy to data structures, and vice-versa.
2. Objects are used when we want to add new data types.
3. Data structures are used when we want to add new functions within the same data.
4. Express your data in an abstract way, keep focused on general ideas, rather than implementation i.e. `datestamp` instead of `timeInDaysAndMonths`. Don't add getters and setters with indifference.

Let's go directly to issues: First, avoid blend data structures and objects, they mix the worst from the two worlds. Second, don't try to treat the active Record as an object, you could create separate objects that contains the business rules and that hide internal data.

To help us to write objects and data structures in a cleaner way, we could use the law of Demeter, which tells us that a method `f` of a class `C` should only call the methods of these: 

- `C`
- An object created by `f`
- An object passed as an argument to `f`
- An object held in an instance variable of `C`

Train wrecks code: 

```python
# This is a Train Wreck:
outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath()

# Use this kind of structure instead:
opts = ctxt.getOptions()
scratchDir = opts.getScratchDir()
outputDir = scratchDir.getAbsolutePath()
```

# Error handling

Error handling is absolutely necessary for all programmers, the reason is very apparent: all things can go wrong in programming. In fact, error handling is important but, it mustn't obscure code logic. Here we have some tips that can help us to write clean and robust code.

1. Use exceptions rather than return codes: Using exceptions helps to unclutter the code. In addition to that we could say that return codes are harder to deal and can be more damaging if we forgot something.
2. When writing methods of functions always try to write your Try-Catch-Finally statement first to ensure good operations, It's a good measure to write tests that force exceptions, and of course, add behaviours to your handler to satisfy your tests.
3. Always try to provide context within exceptions. With this simple act, we can create informative error messages, mention the operation that failed and the type of failure. 
4. When defining exception classes, handle them according to the caller, that means, handle them by how they are caught. With this approach, we can encapsulate errors more appropriately.
5. When organizing your code: business logic comes first, error detection at the edges. It's a good idea to wrap external APIs to throw their own exceptions, you could also create a class or configure an object so that it handles a special case for you.
6. Finally but not least important, DO NOT RETURN OR PASS NULL, because returning or passing null can cause more errors than ever in your codebase.

# Boundaries

Cleanly integrate foreign code with our own, this is all about this section. Here, we will see the best practices and techniques to keep the boundaries of our software clean. We know (or at least should know) that good software designs must accommodate changes without huge investments and\or work. 

When using third-party code, we first should avoid letting too much of our code know about third-party particulars, which means we could manage third-party boundaries by having very few places that refer to them or even define some specific interface to some external code, with only must-have things.

When learning third-party code, do not try to integrate it right away with your production code. One alternative approach to this could be writing some tests to explore our understanding of the third-party code (controlled experiments). Not only are learning tests free, but they also have a positive return on investment. Without these boundary tests to ease an eventual migration, we might be tempted to stay with the old version longer than we should.

One final tip, sometimes choose to look farther from the boundary (unknown knowledge and skills about an API or some third-party code), create some interfaces and test a lot.

# Unit tests

Test code is just as important as production code, it requires thought, design, care and must be kept as clean as production code because having dirty tests is equivalent to, if not worse than having no tests at all. Think about it, the dirtier the tests, the harder they are to change. When more tangled is the test code, more likely you will spend more time cramming new tests. If an old test breaks could be a potential disaster finding and correcting errors.

Unit tests make our code flexible maintainable and reusable, the reason for that is fairly simple: If you have tests, you can make changes freely to the code, test enable changes. Although we have one more question in the air, what does make a test clean? The answer: READABILITY.

The Test Driven Development (TDD) has three laws to help us writing tests for our code:

1. You may not write production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing
3. You may not write more production code than is sufficient to pass the currently failing test.

I think that the reader could see that this methodology could be a potential crammer in test code, in a month we could easily hundreds or even more tests, so we have a tip to help everything write better test code: Test one concept per testing function. If you have a big problem, break it down into small concepts and test them one-by-one, during this process try to minimize the number of asserts in a test function.

One final word about testing: Try to obey the (FIRST) concepts: write **Fast** tests, that work **Independent** from each other (if one of them fails, none other will be affected), that also could be **Repeated** in different environments, not forgetting that tests should have a boolean output, that means, they should be **Self-validating**, and finally, They should be **Timely**, with implies that they ought to be written just before the production code that makes them pass.

# Classes

Following the newspaper metaphor, we should make our classes easy to read. The first thing that comes to our mind is: Classes should be small, but wait for a second, how small a class should be? Here we have some tips when writing or designing your our classes:

- Count responsibilities instead of a number of lines. A class should only have one responsibility (The single responsibility principle), when naming a class this class name describes what responsibilities it fulfils. This design reinforces that a class or a module should have one and only one reason to change.
- Classes should have a small number of instance variables (Cohesion). The methods and variables of the class are co-dependent and hang together as a logical whole, that means that variables should be shared between methods as much as possible. When classes lose cohesion, split them into other classes!
- When refactoring classes, organize them to support the Open-Closed Principle: Classes should be open for extension but not closed for modification, your classes should be designed to be extensible and also for support eventual modifications.
- Dependency Inversion Principle: Our classes should depend upon abstractions, not on concrete details. Think about it, a class that depends on concrete details is at risk when those details change, and they eventually change.

# Systems

Systems must be clean too including all the levels of abstraction, and the system intent should be clear. This will only happen if you write plain objects and you use aspect-like mechanisms to incorporate other implementation concerns non invasively. Remember, whether you are designing systems or individual modules, never forget to use the simplest thing that can possibly work. One thing that always works is thinking about the separation of concerns in an application, which means separate the parts that deal with different concepts in the application (database, interaction with the user, ...). Is one of the oldest and most important design techniques in our craft.

When thinking about the separation of concerns we could move all aspects of construction to the main file, main module, and design the rest of the system assuming that all objects have been constructed and wired up appropriately. The main function could build the objects necessary for the system and then passes them to the application. The application will be only responsible for using these created objects, this is well known as the FACTORY PATTERN.

An optimal system architecture consists of modularized domains of concern, all of them implemented using objects. The different domains (interaction with the user, interaction with the database, ...) are integrated together with minimally invasive aspects. This type of architecture should be test-driven, just like the code inside it. This kind of architecture separated by concerns allows programmers to make optimal, just-in-time decisions, based on most recent knowledge, we can also include the reduced complexity of decisions into this package.

These kinds of standards make it easier to reuse ideas and components, recruit people with relevance experience, encapsulate good ideas and wire components together. However, the process of creating standards can sometimes take too long for the industry to wait, and some standards lose touch with the real need of the adopters they are intended to serve.

# Emergence

Kent Beck's four rules of Simple Design:

- Runs all the tests: A design must produce a system that acts as intended. Testing a system consistently leads to classes that respect the SRP (Single Responsibility Principle) and things like DIP (Dependency Inversion Principle) could emerge, then improving our designs.
- Contains no duplication (Refactoring): Duplication represents additional work, additional risk, and add unnecessary complexity. We could see two kinds of duplication: strict line duplication, and duplication of implementation. We could use the TEMPLATE METHOD PATTERN to remove higher level duplication.
- Expresses the intent of the programmer (Refactoring): Take care of your functions and classes as well as your tests. You can express yourself by choosing good names, by keeping your functions and classes small, by using standard nomenclature i.e. with design patterns like COMMAND or VISITOR. Unit tests must be also expressive, a primary goal of tests is to act as documentation by example.
- Minimizes the number of classes and methods (Refactoring): Our goal is to keep our overall system small while also keeping our functions and classes small, however, this rule is the lowest priority of all, it's only here because high class and method counts are sometimes the results of pointless dogmatism.

# Concurrency

Concurrent code is difficult to get right. If you are faced with writing concurrent code, you need to write clean code with rigour or else face subtle and infrequent failures. Follow the Single Responsibility Principle, Break your system into Plain Objects that separate thread-aware code from thread-ignorant code. When you are testing your thread-aware code, just test only the thread-aware code. Your thread-aware code should be small and focused.

Know the possible sources of concurrency issues, boundary cases. Also, learn your library and know the fundamental algorithms, learn how to find regions of code that must be locked and lock them. Keep the number of shared objects and the scope of the sharing a narrow as possible, change the designs of the objects with shared data to accommodate clients rather than forcing clients to manage the shared state.

Issues will crop up, including one-offs. You also need to be able to run your thread-related code in many configurations on many platforms repeatedly and continuously. Testability which comes naturally from following the Three Laws of TDD implies some level of plug-ability.

You will greatly improve your chances of finding erroneous code if you take the time to instrument your code. It's a good idea to use some kind of automated technology. If you take a clean approach, your chances of getting it right increase drastically.

# Successive Refinement

The key for this chapter is a thing called incrementalism, incrementalism refers to the process of keeping cleaning and improving the code as you continuously write it. One thought that every programmer must keep with, is it's not enough for code to work because code that often just works is bad code and needed to be cleaned.

Talking about bad code, cleaning bad code is a very daunting task, this takes an immense amount of time and effort. Although keep the code clean is very easy, within some minutes you can keep a new functionality clean and working as well.  So take the tip: keep your code as clean and simple as it can be, and never let the mess get started. 

Some things can help us to keep always writing clean code, the first is TDD (Test-Driven Development) which simply told us that you must write tests for your code to be certain that every improvement that you've made don't affect any functionality or breaks up some parts of the code. Another thing that we must observe is the separation of concerns, which told us that every function or class should be responsible for one concern, which again makes the code simple to maintain. And finally but at least, always try to break up the code into pieces that are so much easier to maintain. 

# Smells and Heuristics