---
title: Coding Principles
date: 2023-01-17 9:56:00 +03:00
categories: [Software Development, Design Pattern]
tags: [solid,kiss,dry]
---
# Software Design Principles
Software Design principles are a set of guidelines that helps developers to make a good system design.

One of the most important principle is SOLID principle.

The Key software design principles are as:

## SOLID
It is combination of five basic designing principles.

### Single Responsibility Principle (SRP)
----
This principle states that there should never be more than one reason for a
class to change. This means that you should design your classes in such a way that each class should have a single purpose.

Example - An Account class is responsible for managing  Current and Saving Account but a CurrentAccount and a SavingAccount classes would be responsible for managing current and saving accounts respectively. Hence both are responsible for single purpose only. Hence we are moving towards specialization.

### Open/Closed Principle (OCP)
----
This principle states that software entities (classes, modules, functions, etc.)
should be open for extension but closed for modification. The "closed" part of
the rule states that once a module has been developed and tested, the code
should only be changed to correct bugs. The "open" part says that you should
be able to extend existing code in order to introduce new functionality.

Example – A PaymentGateway base class contains all basic payment related
  properties and methods. This class can be extended by different PaymentGateway
  classes for different payment gateway vendors to achieve theirs functionalities.
  Hence it is open for extension but closed for modification.

### Liskov Substitution Principle (LSP)
----
This principle states that functions that use pointers or references to base
classes must be able to use objects of derived classes without knowing it.

Example - Assume that you have an inheritance hierarchy with Person and Student. Wherever you can use Person, you should also be able to use a Student, because Student is a subclass of Person.

### Interface Segregation Principle (ISP)
---
This principle states that Clients should not be forced to depend upon
interfaces that they don’t use. This means the number of members in the
interface that is visible to the dependent class should be minimized.

Example - The service interface that is exposed to the client should contains
  only client related methods not all.

### Dependency Inversion Principle (DIP)
---
It states that
- High level modules should not depend upon low level modules. Both should depend
upon abstractions.
- Abstractions should not depend upon details. Details should depend upon
abstractions.

It helps us to develop loosely couple code by ensuring that high-level modules
depend on abstractions rather than concrete implementations of lower-level modules.
The Dependency Injection pattern is an implementation of this principle.

Example- The Dependency Injection pattern is an implementation of this principle

---
## DRY (Don’t Repeat Yourself)
---
This principle states that each small pieces of knowledge (code) may only
occur exactly once in the entire system. This helps us to write scalable,
maintainable and reusable code.

If need for repition occurs, create a function and call it repeatedly.

Example – Asp.Net MVC framework works on this principle.

---
## KISS (Keep it simple, Stupid!)
----
This principle states that try to keep each small piece of software simple and
unnecessary complexity should be avoided. This helps us to write easy,
maintainable code.

---
## YAGNI (You aint gonna need it)
-----
This principle states that always implement things when you actually need
them. Never implements things before you need them.



```javascript
console.log('Thank you!');
````
