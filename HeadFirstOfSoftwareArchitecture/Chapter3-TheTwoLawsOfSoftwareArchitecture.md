# Communicating with downstream services 

- use messaging queues or topics to communicate with downstream services

**QUEUES** (point-to-point communication protocol) are a good choice when you need to ensure that a message is processed exactly once. 

- if it helps, think of queues as being like a group text—you pick everyone you want to inform, type your message, and hit “send”. 

_Pros:_
- Supports heterogeneous(different) messages for different onsumers
- Allows independent monitoring and scaling (helps scalability)
- More secure (improves security)

_Cons:_
- Higher degree of coupling (hurts extensibility)
- Trading service must connect to multiple queues
- Requires additional infrastructure

**TOPICS** (publish/subscribe communication protocol) are a good choice when you need to ensure that a message is processed by multiple consumers.
- The publisher simply produces and sends a message. If another service downstream wants to hear from the publisher, it can subscribe to the topic to receive messages.
- Topics are similar to posting a picture on your go-to social networking site. Anyone following you will see that picture, since they’ve “subscribed” to your timeline.

_Pros:_
- Low coupling (helps extensibility)
- Trading service only has one place to publish messages

_Cons:_
- Less secure (hurts security)
- More difficult to monitor and scale
- Homogeneous message for all services

---

# The Two Laws of Software Architecture

1. **EVERYTHING IN SOFTWARE ARCHITECTURE IS A TRADE-OFF**

- *Trade-offs* are the _pros_ and _cons_ you evaluate as you are making a decision.

- Decisions that involve significant trade-offs require much more time and analysis to make and tend to be more architectural in nature.

- Decisions that have less-significant trade-offs can be made quicker, with less analysis, and therefore tend to be more on the design side.

Using SYNC communications (HTTP)
- SYNC pros: consistency, error handling, transactionality
- SYNC cons: extensibility, fault tolerance, responsiveness

Using SYNC communications (messaging queues or topics)
- ASYNC pros: extensibility, fault tolerance, responsiveness
- ASYNC cons: consistency, error handling, transactionality

2. **WHY is more important than HOW**

- **WHY** is the _problem domain_.
- **HOW** is the _solution domain_.

--- 

# Architectural Decision Records (ADRs)

- ADRs are a way to document the architectural decisions you make.
- They are a way to communicate the _why_ behind the _how_.
- ADRs are a way to document the _trade-offs_ you made.
- ADRs are records of a system, including how it’s structured and how it works,that anyone can review at any time
- Over time, your ADRs will build into a log of architectural decisions that will serve as the memory store of your project.

Strcutre of an ADR:

1. **Title** (001: short description of the decision)
- should consist of a three-digit numerical prefix and a noun-heavy, succinct description of the decision being made

2. **Status** (Request for Comments, Proposed, Accepted, Superseded)

- _Request for Comments:_ the decision is being discussed
- _Proposed:_ the decision has been made but not yet implemented
- _Accepted:_ the decision has been made and implemented
- _Superseded:_ the decision has been replaced by another decision
	- if a decision is superseded, the new decision should reference the old decision
	

3. **Context**
- What is the problem you are trying to solve?

4. **Decision**
- What is the decision you made?

5. **Consequences**
- What are the trade-offs you made?

6. **Governance** (optional)
- outline how you’ll ensure that your organization doesn’t deviate from the decision — now or in the future.

7. **Notes** (optional)
- the Notes section contains metadata about the ADR itself (e.g., original author, approval date, approved by, superseded by, last modified date, modified by, last modification)

