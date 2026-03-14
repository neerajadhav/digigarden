---
title: "Case Study: Designing a Document Editor"
tags:
  - DesignPattern
---

We are designing a WYSIWYG (What You See Is What You Get) Editor called ***Lexi***. 

## Design Problems
We will examine 7 problems in Lexi's design.

### 1. Document Structure
The choice of internal representation for the document affects nearly every aspect of Lexi's design. All editing, formatting, displaying, and textual analysis, will require traversing the representation. The way we organize this information will impact the design of the rest of the application.

### Formatting
How does Lexi actually arrange text and graphics into line and columns? What objects are responsible for carrying out different formatting policies? How do these policies interact with the document's internal representation?

### Embellishing the User Interface
Lexi's UI includes scroll bars, borders, and drop shadows that embellish the WYSIWYG document interface. Such embellishments are likely to change as Lexi's UI evolves. Hence it's important to be able to add or remove embellishments easily without affecting the rest of the application.

### Supporting Multiple Look and Feel Standards
Lexi should adapt easily to different look-and-feel standards such as Motif and Presentation Manager without major modifications.

### Supporting Multiple Window Systems
Different look-and-feel standards are usually implemented on different window systems. Lexi's design should be as independent of the window system as possible.

### User Operations
Users control Lexi through various user interfaces, including buttons and pull-down menus. The functionality behind these interfaces is scattered throughout the objects in the application. The challenge here is to provide a uniform mechanism for both accessing and undoing the effects of these scattered functionalities.

### Spelling checking and Hyphenation
How does Lexi support analytical operations such as checking for misspelled words and determining hyphenation points. How can we minimize the number of classes we have to modify to add a new analytical operation?

