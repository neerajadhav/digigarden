---
title: 01. Introduction
tags:
  - DesignPattern
---

(From **Design Patterns: Elements of Reusable Object-Oriented Software**)


## **1.1 What Is a Design Pattern?**

### Definition

A **design pattern** is a **general, reusable solution** to a **commonly occurring problem** in software design within a given context.

### Key Characteristics

A design pattern:

- Is **not code**, but a **template/blueprint**
    
- Solves a **design problem**, not an implementation detail
    
- Is **language-independent**
    
- Is **proven** through repeated use
    
- Improves **communication** among developers
    

### What a Design Pattern Is NOT

- Not an algorithm
    
- Not a finished design
    
- Not a framework
    
- Not a library
    

### Why Design Patterns Matter

- Promote **reusability**
    
- Improve **maintainability**
    
- Reduce **coupling**
    
- Increase **flexibility**
    
- Provide a **shared vocabulary** (e.g., “use Observer here”)
    


## **1.2 Design Patterns in Smalltalk MVC**

### MVC (Model–View–Controller)

MVC is an **early example** of a design pattern system used in **Smalltalk**.

### Components

- **Model**
    
    - Represents core data and business logic
        
- **View**
    
    - Displays data to the user
        
- **Controller**
    
    - Handles user input and updates the model
        

### Design Patterns Inside MVC

MVC itself is not a single pattern but a **combination of patterns**:

- **Observer**
    
    - Views observe the Model for changes
        
- **Strategy**
    
    - Controllers act as interchangeable strategies
        
- **Composite**
    
    - Views can be nested
        

### Why MVC Is Important

- Demonstrates how **multiple patterns collaborate**
    
- Encourages **separation of concerns**
    
- Influenced modern frameworks (Spring MVC, Django, Rails)
    

## **1.3 Describing Design Patterns**

GoF uses a **standard template** to describe every pattern.

### Pattern Description Elements

1. **Pattern Name**
    
    - A handle to communicate design ideas
        
2. **Intent**
    
    - What the pattern does and why
        
3. **Motivation**
    
    - Problem scenario explaining need
        
4. **Applicability**
    
    - When to use the pattern
        
5. **Structure**
    
    - UML-style class relationships
        
6. **Participants**
    
    - Classes/objects involved
        
7. **Collaborations**
    
    - How participants interact
        
8. **Consequences**
    
    - Trade-offs (flexibility vs complexity)
        
9. **Implementation**
    
    - Pitfalls and tips
        
10. **Sample Code**
    
11. **Known Uses**
    
12. **Related Patterns**
    

### Interview Tip

If asked to explain a pattern, **Intent + Applicability + Consequences** is usually sufficient.

## **1.4 The Catalog of Design Patterns**

The GoF catalog contains **23 design patterns**.

### Purpose of the Catalog

- Provide **standard solutions**
    
- Avoid reinventing the wheel
    
- Encourage **best practices**
    

### Pattern Scope

Patterns differ by:

- **Purpose** (what they do)
    
- **Scope** (class vs object)
    

## **1.5 Organizing the Catalog**

### Classification by Purpose

1. **Creational Patterns**
    
    - Object creation mechanisms
        
    - Example: Factory, Singleton
        
2. **Structural Patterns**
    
    - Class/object composition
        
    - Example: Adapter, Decorator
        
3. **Behavioral Patterns**
    
    - Object interaction and responsibility
        
    - Example: Observer, Strategy
        

### Classification by Scope

|Scope|Meaning|
|---|---|
|Class|Uses inheritance|
|Object|Uses composition|

### Why This Organization Helps

- Makes pattern selection easier
    
- Highlights **design intent**
    
- Clarifies trade-offs

## **1.6 How Design Patterns Solve Design Problems**

### Core Design Problems Addressed

1. **Finding Appropriate Objects**
    
    - Identify responsibilities, not classes
        
2. **Determining Object Granularity**
    
    - Avoid God objects
        
3. **Specifying Object Interfaces**
    
    - Program to interfaces, not implementations
        
4. **Managing Dependencies**
    
    - Reduce tight coupling
        
5. **Making Code Reusable**
    
    - Favor composition over inheritance
        

### Fundamental OO Principles Used

- Encapsulation
    
- Abstraction
    
- Polymorphism
    
- Composition over inheritance
    
- Loose coupling
    

## **1.7 How to Select a Design Pattern**

### Steps to Choose a Pattern

1. **Understand the problem**
    
2. **Study pattern intents**
    
3. **Consider causes of redesign**
    
4. **Examine patterns with similar intent**
    
5. **Evaluate consequences**
    
6. **Consider scalability and flexibility**
    

### Interview Rule of Thumb

Do **not** force patterns.  
Use them **only when the problem context fits**.


## **1.8 How to Use a Design Pattern**

### Steps to Apply a Pattern

1. **Read the pattern completely**
    
2. **Understand its intent and structure**
    
3. **Identify participants in your design**
    
4. **Adapt names and structure**
    
5. **Implement incrementally**
    
6. **Refactor if needed**
    

### Common Mistakes

- Overengineering
    
- Blindly applying patterns
    
- Using patterns without understanding trade-offs
    
## **Quick Interview Summary (1-Minute Revision)**

- Design patterns are **reusable design solutions**
    
- GoF catalog contains **23 patterns**
    
- MVC demonstrates **pattern collaboration**
    
- Patterns are described using a **standard template**
    
- Classified into **Creational, Structural, Behavioral**
    
- Patterns improve **flexibility, maintainability, communication**
    
- Use patterns **judiciously**, not everywhere
    
