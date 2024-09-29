# Basic Concepts
<p>Software Architecture is less about appearance and more about structure. There are four dimensions to understand and descrive software architecture: </p>

1. **Architectural Characteristics** 
2. **Architectural Decisions**
3. **Logical Components**
4. **Architectural Style** 

<br>

1. **Architectural Characteristics** ("ilities") - describes what aspects of the system the architecture needs to support:

    - _sacalability_: the system's ability to maintain a consistent response time and error rate as the number of users or request increases.

    - _availability_: the amount of uptime of a system; usually measured in "nines" (99.9% would mean thee nines).

    - _agility_: respond quickly to change (a function of maintainability, testability and deployability).

    - _elasticity_: handling thousands of concurrent users

    - _feasibility_: taking into account time frames, budgets, and developer skills when making architectural decisions. 

    - _interoperability_: interacting and iterfacing with many external systems to complete a business request.

    - _testability_

    - _performance_: the amount of time it takes for the system to process a business report.

    - _faut tolerance_: the system's ability to keep its other parts functioning when fatal errors occur.

**Example:** the product owner insists that we get new features and bug fixes out to our customers as fast as possible (agility).

<br>

2. **Architectural Decisions** - includes important decisions that have **long-term** or significant implications for the system:
    - the kind of database used
    - the number of services
    - how these services communicate with each other


**Example:** 
- the UI shall not communicate directly with the database.
- the single payment service will be broken appart into separate services, one for each payment type we accept. 

<br>


3. **Logical Components** - describes the **building blocks** of the system's functionality and how they interact with each other.

**Example:** we are going to start offerring rewards points as a new payment option when paying for an order -> this is about adding a new logical component to the architecture.

**Question:** What is the difference between the system functionality and the domain?
- the domain (the "what") is the problem you are trying to solve
- the system functionality (the "how") is how you are solbing that problem

<br>

4. **Architectural Style** - defines the overall **physical shape and structure** of a software system.
    - Microservices (agility)
    - Layered architecture (less complex and less costly)
    - Event-driven architecture (high scalability)


# Desing vs Architecure

**DESIGN** says nothing about the _physical structure_ of the source code (how the classes will be organised and deployed). It can use **UML** (Unified Modeling Language) class diagram that shows how classes _interact_ with each other. 

<br>

**ARCHITECTURE** is about the _structure_ of the system.
- how services communicate with each other and the UI
- which service can access which databse
- how many services and databases there are

<br>

**The SPECTRUM between architecture and design**
In reality, most decisions you encounter will fall between these two examples, within a spectrum of architecture and design. Knowing where along the spectrum between architecture and design your decision lies helps determine who should be responsible for ultimately making that decision.

- breaking aparat a service
- selecting a UI framework
- choosing a persistence framework
- deciding to use a graph database

<br>

####_Where along the spectrum does your decision fall?_

1. **Is it strategic or tactical?**

**STRATEGIC DECISIONS** are long-term and influence future actions or decisions. The more strategic the decision, the more it stis toward the architecture side of the spectrum.

**TACTICAL DECISIONS** are short-term and generally stand independent of other actions or decisions. 


<br>

2. **How much effort will it take to construct or change?**

- architectural decisions require more effort to construct or change. 


<br>


3. **Does it have significant trade-offs?**

**TRADE-OFFS** are the _pros_ and _cons_ you evaluate as you are making a decision.

Decisions that involve significant trade-offs require much more time and analysis to make and tend to be more architectural in nature. Decisions that have less-significant trade-offs can be made quicker, with less analysis, and therefore tend to be more on the design side.



####_How can I determine whether a decision is more strategic or tactical?_

1. How much thought and planning do you need to put into the decision?
- a couple of minutes/up to an hour -> tactical
- several days/weeks -> strategical

<br>


2. How many people are involved in the decision?
- less people -> tactical
- more people -> strategical


<br>


3. Does your decision involve a long-term vision or a
short-term action?


**ARCHITECTURE examples:**
- migrating your system to a cloud environmnet
- replacing your UI framework
- moving from a relational to a graph database

<br>

**DESIGN examples:**
- resolving a merge conflict in Git
- breaking apart a class file
- renaming a method or function

<br>

**MIDDLE:**
- Breaking apart a single service into separate ones


<br>


**Significant trade-offs:** these can impact scalability, performance and overall maintainability
- deciding which architectural style to use
- choosing between REST and messaging
- choosing to deploy in the cloud or on premisis
- using full data or only keys for the me ssage payload
- choosing between atomic or distributed transactions (This can impact data integrity and data consistency, but also scalability and performance.)
- deciding whether or not to break apart a service
- selecting a user interface framework (it depends)


<br>


**NOT significant trade-offs:**
- selecting a user interface framework (it depends)
- deciding on the name of a variable in a class file
- selecting an XML parsing library

