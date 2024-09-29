# Basic Concepts
<p>Software Architecture is less about appearance and more about structure.</p>

There are four dimensions to understand and descrive software architecture:
1. **Architectural Characteristics** 
2. **Architectural Decisions**
3. **Logical Components**
4. **Architectural Style** 

------

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

-----

2. **Architectural Decisions** - includes important decisions that have **long-term** or significant implications for the system:
    - the kind of database used
    - the number of services
    - how these services communicate with each other

3. **Logical Components** - describes the **building blocks** of the system's functionality and how they interact with each other.

4. **Architectural Style** - defines the overall **physical shape and structure** of a software system.
    - Microservices (agility)
    - Layered architecture (less complex and less costly)
    - Event-driven architecture (high scalability)

