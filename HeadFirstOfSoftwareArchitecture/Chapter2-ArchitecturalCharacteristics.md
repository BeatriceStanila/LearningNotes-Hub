_Architectural characteristics_ (the nonfunctional requirments) and _logical components_ before you can choose an _architectural style_ (the decision we make after doing a trade-off analysis). _Architectural decisions_ represent the problem domain.

## What are architectural characteristics? "ility"

An architect has to structural design software systems, for which there are two parts:

- Logical components - the domain of the application.

- Architectural characteristics - the important parts of the construction process of a software system or application, regardless of the problem domain (non-domain).

---

The architectural characteristics can be defined in three parts 🔺:

1. **Nondomain design considerations (explicit)**

- the _requirments_ specify what the application should do
- we need to work with the BA and others experts as early on as possible to create the requirments docuemnt in order to find out what architectural characteristics we need to support

2. **Influences some structural aspect of the design (implicit)**

 - you must consider many trade-offs when making architectural decisions, such as whether to use a monolithic versus distributed physical architecture.

3. **Crictical or important to application success**

Try to pick as few architectural characteristics as possible, rather than as many as possible. This is because architectural characteristics are:

- impossible to standardize: different organizations use different terms for the same architectural
characteristics; for example, performance and responsiveness might indicate the same behavior.

- synergistic: you often cannot choose one architectural characteristic without considering how it may affect others.

- overabundant: possible architectural characteristics are extraordinarily abundant, and new ones appear all the time; for example, a few years ago there was no such thing as on-demand elasticity via a cloud provider

---

## Explicit vs Implicit

**Explicit** architectural characteristics are specified in the requirements for the application.
- we must support hundreds of users

**Implicit** architectural characteristics are factors that influence an architect’s decisions but aren’t explicitly called out in the requirements.
- we need to protect the privacy of users

- architects should pay attention to the application’s internal structure as developers create it, to ensure that sloppy coding and other deficiencies don’t degrade the longevity of the application.

---
## Zoo "-ilities"

1. **Process architectural characteristics** - where the software development process intersects with software architecture. 


- _agility_ (_modularity_, _testability_, _deployability_, and others)- facilitates and enables agile software development practices.
- _modularity_ - the degree to which the software is composed of discrete components; affects how architects partition behaviour and organize loigcal building blocks.
- _testability_  refers to testing at development time (unit testing) rather than formal quality assurance.
- _deployability_ - how easy and efficient it is to deploy the software system.
- _extensibility_ - how easy it is for developers to extend the system. This may encompass architectural structure, engineering practices, internal design, and governance.
- _decouple-ability (coupling)_ - describes how parts of the system are joined together. Some architectures define how to _decouple_  parts in specific ways to achieve certain benefits;

--- 
2. **Strcutural architectural characteristics** - affects the internal structure of the software system; including factors like the degree of coupling between components and relationships between different integration points.

- _security_ - appears in every application as an implicit or explicit characterisitc. 
- _maintainability_ - how easy it is for architects and developers to apply changes to enhance the system and or fix bugs.
- _portability_ - how easy it is to run the system on more than one platform (for example, Windows and macOS); can be applied to any part of the system (UI and implementation platform).
- _extensibility_ - this belongs to more then one category.
- _localization_ - how well the system supports multiple languages, units of measurement, currencies and other factors that allow it to be used globally. 

---
3. **Operation architectural characteristics** -represent how architectural decisions influence what operation team members can do. 

- _availability_ - what percentage of the time the system needs to be available and, if 24/7, how easy it is to get the system up and running quickly after a failure.
- _recoverability_ - how quickly the system can get oline again and maintain business continuity in case of a disaster; this will affect the backup strategy and requirements for duplicated hardware. 
- _robustness_ - the system's ability to handles errors and boundary conditions while running (if the power, internet connection or hardware fails).
- _performance_ - how well the system achiebes its timing requirements using the available resources.
- _reliability/safety_ - whether the system needs to be fail-safe, or if it is mission critical in a way that affects lives. If it fails, will it endanger people’s lives or cost the company large sums of money? Common for medical systems, hospital software, and airplane applications.
- _scalability_ - how well the system performs and operates as the number of users or request increases. 

---
4. **Cross-cutting architectural characteristics** 

- _security_
- _authentication/authorization_ (part of security)
- _legal_ - how well the system complies with local laws about data protection and about how the application should be built or deployed.
- _privacy_ - how well the system hides and encrypts transactions so that internal employees like data operators,architects, and developers cannot see them.
- _accessibility_ - how easy is it for all your users to access the system, including those with disabilities, like colorblindness or hearing loss.
- _usability_ - How easy is it for users to achieve their goals. Is training required? Usability
requirements need to be treated as seriously as any other architectural issue. 

--- 

**Can I choose any combination of architectural characteristics for my application?**
<p>Some architectural characteristics oppose one another. For example, architects find it challenging to design for both high performance and scalability. Determining the most important architectural characteristics for a system is only part of the design process. Combining them with logical component design will point you to an appropriate architectural style.</p>

--- 

There are 3 different sources that can help us find the architectural characteristic critical to our project:

1. **Sourcing Architectural Characteristics from the Problem Domain:** Architects must understand the specific needs of the problem they are solving to design the right system. This involves translating requirements like handling thousands of users into technical solutions such as scalability (handling large numbers), concurrency (handling many users at once), and elasticity (adjusting to fluctuating loads).

2. **Sourcing Architectural Characteristics from Environmental Awareness:** Architects also need to be aware of the organizational environment they are working in. Priorities like speed or mergers may affect their decisions. It’s important to consider these contextual factors, rather than making decisions in isolation, to ensure the design fits well with the business goals.

3. **Sourcing Architectural Characteristics from Holistic Domain Knowledge:** Architects use their knowledge of the domain to anticipate challenges that may not be explicitly stated. For example, understanding how students behave during a sign-up event can help design a system that handles bursts of activity effectively, ensuring the solution is practical based on real-world patterns.

---

Architectural characteristics ~ capabilities
- describe the kinds of capabilities your solution will support, rather than the behavior of the application, which is based on requirements.

Logical components = behaviour 
- representthe design of the system you are attempting to implement in software in order to solve the fundamental problem at hand.