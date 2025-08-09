# Complete Code Summary

## 1. Laying the Foundation

### Architecture Principles
- **Single Responsibility**: Each building block should have one area of responsibility and know as little as possible about other building blocks' responsibilities
- **80/20 Rule**: Specify the 20% of classes that make up 80% of the system's behavior

### Key Design Areas

#### Data Design
Data should normally be accessed directly by only one subsystem or class, except through access classes or routines that allow controlled and abstract access.

#### Interface Design
The architecture should be modularized so that a new user interface can be substituted without affecting the business rules and output parts of the program.

#### Scalability
Architecture should describe how the system will address growth in:
- Number of users
- Number of servers
- Network nodes
- Database records and size
- Transaction volume

#### Interoperability
If the system is expected to share data or resources with other software or hardware, the architecture should describe how that will be accomplished.

#### Error Processing
- **Corrective**: System attempts to correct the error
- **Detective**: Continue as nothing happened or quit
- **Active vs Passive**: Actively anticipate errors vs passively respond when errors cannot be avoided

#### Overengineering
Set expectations explicitly in the architecture to avoid inconsistency where some classes are exceptionally robust and others are barely adequate.

## 2. Key Construction Decisions

### Design Philosophy
You have to "solve" the problem once to clearly define it, then solve it again to create a solution that works.

### Complexity Management
- Minimize the amount of essential complexity that anyone's brain has to deal with at any one time
- Keep accidental complexity from needlessly proliferating

### Characteristics of Good Design
- **Minimal complexity**: Easy to understand design; avoid clever designs that are hard to understand
- **Ease of maintenance**
- **Loose coupling**: Hold connections among different parts of a program to a minimum
- **Extensibility**: Enhance a system without causing violence to the underlying structure

### Levels of Design

#### Division into Subsystems or Packages
- Have acyclical (not cyclical) relationships among subsystems
- Simple relationships:
  - One subsystem calls another
  - One subsystem contains classes from another
  - Class in one subsystem inherits from one in another
- Common systems: business rules, user interface, database access, system dependencies

### Design Concepts

#### Inheritance
Defining similarities and differences among objects.

#### Information Hiding
- Avoid excessive distribution of information
- Put constants into one class to be accessed by generic name

#### Anticipating Changes
Find the core subset of the system that is unlikely to change, then identify add-ons.

#### Coupling
- **Loose Coupling**: Good coupling between modules allows one module to be easily used by other modules
- **Semantic Coupling**: The most insidious kind occurs when one module uses semantic knowledge of another module's inner workings

#### Design Patterns
**Factory Method**: Allows you to instantiate any class derived from a specific base class without needing to track individual derived classes anywhere but the Factory Method.

#### Cohesion
Each class should support a central purpose.

#### Development Approaches
- **Top-down**: Humans can only focus on one thing at a time. It's iterative as you don't settle for the first attempt
- **Bottom-up**: Used when top-down is too abstract and you want something more tangible to start with

## 3. Creating High-Quality Code

### Class Design
- Minimize the extent to which a class collaborates with other classes
- Avoid creating well-rounded classes
- Aim for strong cohesion

### High-Quality Routines
- **Primary Purpose**: Reduce complexity
- Create routines to hide information so you won't need to think about it

#### Robustness vs Correctness
- **Robustness**: Always return something to keep things working
- **Correctness**: Only return correct results or don't return anything at all
- **Preference**: Favor correctness over robustness

### Defensive Programming
- **Defensive**: Program against invalid inputs and operator's mistakes
- **Offensive**: Fail hard when an invalid input occurs

### The Pseudocode Programming Process

#### Class Design Steps
1. Identify the class's key public methods
2. Identify and design non-trivial data members used by the class

#### Routine Definition
Define what the routine will solve:
- Input/output
- Information hidden inside
- Preconditions guaranteed to be true before the routine is called
- Postconditions the routine guarantees will be true before returning control

#### Documentation
- Write a concise statement of the routine's purpose
- Trouble writing the general comment warns that you need to understand the routine's role better
- Pseudocode can make assumptions and high-level mistakes more obvious than programming-language code
