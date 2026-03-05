---
title: "Software Architecture: The Hard Parts"
date: 2026-03-05
description: "Neal Ford and Mark Richards' guide to making hard architectural decisions in distributed systems — trade-off analysis, decomposition and data ownership"
tags: ["software-engineering", "architecture", "distributed-systems", "books"]
ShowToc: true
---

**Author:** Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani

**Genre:** Software Engineering, Technology

**Rating:** 4/5

## 🚀 The Book in 3 Sentences

1. The book is a practical guide to making hard architectural decisions in distributed systems when there are no clear best practices and every option is a trade-off
2. It covers patterns and techniques for breaking apart monoliths, managing data ownership, handling distributed transactions and choosing the right level of service granularity
3. Through structured trade-off analysis and real world scenarios, it teaches you to reason about architectural decisions instead of relying on gut feeling

## 🎨 Impressions

The book does not pretend there are silver bullets. It gives you frameworks to reason about trade-offs which is what architecture is really about. The chapters on data ownership and distributed transactions were the most valuable. The pros/cons for every pattern make it easy to reference when you are actually making these decisions at work. Some chapters felt repetitive but that is because the same trade-off dimensions keep showing up in different contexts which reinforces the thinking.

### How I Discovered It

It kept coming up in discussions around microservice decomposition and distributed architecture. Picked it up after reading Fundamentals of Software Architecture by the same authors and wanting to go deeper on the hard decisions.

### Who Should Read It?

Any software engineer or architect working with or transitioning to distributed systems. Especially useful if you are breaking apart a monolith or dealing with data ownership challenges across services.

## ☘️ How the Book Changed Me

- I have a better framework for reasoning about trade-offs instead of defaulting to gut feeling or popular opinion
- I am more deliberate about data ownership decisions and understand the joint ownership patterns
- I think about service granularity as a spectrum with clear drivers rather than just making things as small as possible
- The saga patterns gave me vocabulary to discuss distributed transaction strategies with my team

## ✍️ Quotes

> A pattern is recognition of only commonality and not solvability

<details>
<summary><strong>📒 Summary + Notes</strong></summary>

### Chapter 1. What happens when there are no best practices?

In software architecture there are no best practices, only trade-offs. The key question is not "which is best?" but "which trade-offs can we live with?"

**Architecture decision records (ADRs)** are a lightweight way to document decisions. Every decision should capture the context, the options considered and the trade-offs that led to the chosen option. Without recording decisions the reasoning gets lost over time and teams repeat the same debates.

**Trade-off analysis** is the core skill of an architect. You need to analyse competing dimensions like performance, scalability, data integrity and fault tolerance. If you think an option has no trade-offs you have not looked hard enough.

---

### Chapter 2. Discerning concerns in Software Architecture

This chapter introduces how to identify and separate architectural concerns when analysing a system.

**Architectural drivers** are the combination of requirements, constraints and assumptions that shape architectural decisions. Understanding these is the first step to making good trade-offs.

**Coupling** is the degree to which components depend on each other. Two types of coupling discussed
- Static coupling - How services are wired together, dependencies at compile time or deployment time
- Dynamic coupling - How services call each other at runtime, synchronous vs asynchronous

**Architectural quantum** is an independently deployable artifact with high functional cohesion and synchronous connascence. This is the unit of architecture, the smallest thing you can deploy independently that has meaning. A monolith has a single quantum, everything must be deployed together. Microservices aim for one quantum per service but coupling can create larger quanta in practice.

---

### Chapter 3. Architectural Modularity

The chapter discusses how to measure and evaluate modularity in architecture.

**Modularity drivers** are the reasons you might want to break a system apart. These include maintainability, testability, deployability, scalability, fault tolerance and team autonomy. Not every system benefits from decomposition. If the system works well as a monolith and there are no pressing drivers, the cost of distributed systems may not be worth it.

Before breaking things apart you need to clearly identify which drivers are motivating the change. The more drivers you have the stronger the case for decomposition. Modularity is not a goal in itself, it is a means to achieve specific architectural characteristics.

---

### Chapter 4. Architectural Decomposition

This chapter provides a structured approach to breaking apart monolithic architectures.

- Tactical forking - Clone the entire monolith and start deleting what you do not need. Quick and dirty but works when the codebase is too tangled for surgical extraction
- Component-based decomposition - Identify logical components within the monolith and extract them one at a time. More deliberate and controlled. Works better when the monolith already has some structure

The hardest part of decomposition is not the code, it is the data. Do not try to decompose everything at once, identify the component that gives the most value and start there.

---

### Chapter 5. Component Based Decomposition Pattern

A step-by-step pattern for extracting components from a monolith into services.

1. Identify and size components - Find the logical groupings in the monolith. Too large and you lose the benefit, too small and you increase operational overhead
2. Gather common domain components - Group related functionality that belongs to the same domain
3. Flatten components - Simplify nested or overly hierarchical component structures
4. Determine component dependencies - Map out what depends on what. This reveals the real coupling in the system
5. Create component domains - Group components into domains that represent bounded contexts
6. Create domain services - Extract domains into independently deployable services

Dependencies between components are what make decomposition hard. Pay close attention to database dependencies, they are often the hidden coupling that blocks extraction. Start with the component that has the fewest afferent (incoming) couplings.

---

### Chapter 6. Pulling Apart Operational Data

This chapter focuses on the data side of decomposition, how to break apart shared databases.

**Data disintegrators** are reasons to split data apart. These include change control (shared schemas require coordination across teams), connection management (shared databases create contention), scalability (different data stores may need different scaling strategies), fault tolerance (shared database is a single point of failure) and architectural quanta (to achieve independent deployability the data must also be independent).

**Data integrators** are reasons to keep data together. These include data relationships (foreign keys and joins become distributed queries when split), database transactions (ACID guarantees are lost across boundaries) and data consistency (eventual consistency adds complexity).

Breaking apart data is harder than breaking apart code. This is where most decomposition efforts stall. Always analyse the data relationships before deciding on service boundaries. Sometimes keeping a shared database is the pragmatic choice if the trade-offs of splitting are too costly.

---

### Chapter 7. Service Granularity

This chapter provides a framework for deciding how big or small a service should be.

**Granularity disintegrators** are reasons to make services smaller
- Service scope and function - A service doing too many things should be split
- Code volatility - Parts that change frequently should be separated from stable parts
- Scalability and throughput - Components with different scaling needs should be independent
- Fault tolerance - Isolate components whose failure should not cascade
- Security - Sensitive operations may need their own service boundary
- Extensibility - Parts likely to grow independently benefit from separation

**Granularity integrators** are reasons to keep services larger
- Database transactions - If two operations need ACID guarantees keep them together
- Workflow and choreography - Too many services in a workflow increases complexity and latency
- Shared code - If services share a lot of logic combining them may be simpler
- Data relationships - Tightly related data is easier to manage in one service

Finding the right granularity is a balancing act between these two forces. The "micro" in microservices does not mean small, it means right-sized for purpose. When in doubt start larger and split later when you have evidence of a need.

---

### Chapter 8. Reuse Patterns

This chapter explores patterns for code and functionality reuse in distributed architectures.

- Code replication - Copy the code into each service that needs it. Simple but creates maintenance overhead when the shared logic changes
- Shared library - Extract common code into a library. Works well for utility code but creates coupling when the library changes frequently, all consumers need to update
- Shared service - Create a dedicated service that encapsulates the shared functionality. Adds a runtime dependency but centralises the logic
- Sidecar pattern - Deploy shared functionality alongside each service as a sidecar process. Common in service meshes for cross-cutting concerns like logging, security and circuit breaking. Good decoupling but adds operational complexity

| Pattern | Coupling | Versioning | Performance | Fault tolerance |
| --- | --- | --- | --- | --- |
| Code replication | Low | Easy | Best | Best |
| Shared library | Medium | Moderate | Good | Good |
| Shared service | High | Centralised | Lower | Lower |
| Sidecar | Low | Moderate | Good | Good |

There is no single best reuse pattern. For utility code that rarely changes shared libraries work fine. For domain logic that changes frequently, shared services or code replication may be better depending on coupling tolerance. Sidecar is best for infrastructure concerns not business logic.

---

### Chapter 9. Data ownership and Distributed Transaction

**Assigning data ownership**

This section focuses on where the data should lie in micro service context, and how to deal with domains that are needed beyond the bounded context

The general rule of thumb for data ownership is that service that perform write operation to a table is the owner of that table. However joint ownership makes this simple rule complex.

- Single ownership scenario
This occurs when only one service writes to a table. This is most straightforward ownership. The service that writes to the table owns the table/ domain
- Common ownership scenario
This situation arises when all the service wants to write to the same table. If we create a shared schema or shared database, then problems like change control, schema starvation, scalability and fault tolerance arises.
In such scenario it make sense when multiple service wants to update the entire row of a table to create a dedicate single service which will handle request from all the services that wants to write to this table. This can be done synchronously or asynchronously
Some case we might want to read common data, such read access patterns are discussed in Chapter 10
- Joint ownership scenario
In this case not all the services want to write to the same table, a select few wants to write to it. (I would prefer to understand this as multiple services wants to partially update the data on a domain)
    - Table split technique - This technique breaks the table into multiple tables. However once the service owns the table individually they need to communicate in order to keep data consistent. There are a lot of complications in keeping data consistent. Should we do it sync or async (consistency vs availability)


        Pros:

        - Preserves bounded context
        - Single data ownership

        Cons:

        - Tables must be altered restructured
        - Data consistency issues
        - No ACID transaction between updates
        - Sync is difficult
        - Replication may occur

    - Data domain technique - This technique puts the table shared into a common schema or database. This forms a broader bounded context. Even though sharing database is discouraged it can solve some problems related, performance, scalability and data consistency issues. Both services don't need to communicate anymore.
    This in turn adds back the issues we are avoiding, changing structure becomes more challenging. You need to start co-ordinating on changes with other team, increases testing scope and increases risk in general.


        Pros:

        - Good data access performance
        - No scalability and throughput issue
        - Data remains consistent
        - No service dependency

        Cons:

        - Schema change involves more service
        - Increase testing scope
        - Data ownership governance
        - Deployment risk
    - Delegate Technique - In this technique the one service is assigned ownership of the table and all other services call the owner service to perform operation. There are two options to decide who should be the owner.
        - Primary domain priority - Assign ownership to service that most closely represents the domain, does all the CRUD for entity
        - Operational characteristics priority - Assign it to service that needs higher operational architecture characteristics, like performance, scalability, availability and throughput. Main problem with this approach is domain management responsibility, it needs to handle the crud as well. Hence first option is recommended

        Pros:

        - Forms single table ownership
        - Good data schema change control
        - Abstract data structure from other domain

        Cons:

        - Service coupling
        - Low performance for non-writers
        - Non atomic transaction for non-owner writes
        - Low fault tolerance for non-owner

**Service consolidation Technique**

Service consolidation technique resolve the issues that comes with joint ownership. Although it solves all the problem associated with joint ownership it has it's own trade off

Combining service creates more coarse grained service. Increases testing scope, risk of deployment. Impacts fault tolerance, scalability

Pros:

- Preserves atomic transaction
- Good performance

Cons:

- More coarse grained scalability
- Less fault tolerance
- Increases deployment risk
- Increases testing scope

**Distributed transaction**

Understand ACID transaction to fully understand distributed transaction. Instead we have BASE for distributed transaction

- Basic availability - This means that all the services participating in a distributed transaction will be available. Async can enhance the availability, but can take a lot of time to process business transaction.
- Soft state - This describes a situation when distributed transaction is in progress. Not all services might have completed handling the transaction so the exact state is unknown
- Eventual consistency - This means given enough time data will be in sync given time and transaction successfully completed.

**Eventual consistency patterns**

Distributed architecture relies on eventual consistency for performance, scalability, elasticity, fault tolerance and availability.

- Background synchronisation pattern
This method uses external separate service to periodically check data sources and keep them in sync with one another. Time for data to be consistent varies on the frequency of the process.


    Pros:

    - Preserves atomic transaction
    - Good performance

    Cons:

    - More coarse grained scalability
    - Less fault tolerance
    - Increases deployment risk
    - Increases testing scope
- Orchestrated Request-Based Pattern
This pattern syncs the data during the course of business request. Since it attempts to process the entire transaction we need a designated orchestrator which can be an existing service or a new service.


    Pros:

    - Service are decoupled
    - Timeliness of data consistency
    - Atomic business request

    Cons:

    - Slow responsiveness
    - Complex error handling
    - Usually requires compensating transactions (Rollback failures)
- Event-Based Pattern
Pub/Sub based pattern. Time for consistency is shorter, because of parallel and decoupled nature.


    Pros:

    - Service are decoupled
    - Timeliness of data consistency
    - Fast responsiveness

    Cons:

    - Complex error handling


---

### Chapter 10. Distributed Data Access

When data is broken up into separate database or schema owned by distinct services, data access for read operation starts to become hard.

**Interservice communication pattern**

One of the most common communication pattern.

Pros:

- Simplicity
- No data volume issues

Cons:

- Network, data and security latency (performance)
- Scalability and throughput issues
- No fault tolerance (availability)
- Requires contracts between services

**Column Schema Replication Pattern**

Columns that are needed by other service is replicated on other service table. Main issues are data sync and consistency. Another challenge is data governance. Since data is available in bounded context, it increases performance, fault tolerance and scalability. Can shine when we need data aggregation, reporting, or when other access pattern are not a good fit because of large data volumes, high responsiveness requirement or high fault tolerance.

Pros:

- Good data access performance
- No scalability and throughput issue
- No fault-tolerance issues
- No service dependencies

Cons:

- Data consistency issue
- Data ownership issue
- Data synchronisation required

**Replicated Caching Pattern**

Use distributed caching as data access pattern.

In memory cache for service are good for improving latency but not good for sharing data between service.

In distributed caching the data resides in a separate caching server. Not useful since no fault tolerance. Breaches bounded context. Can cause data inconsistency, data governance issue. Network latency added.

In this access pattern data is replicated in each service cache.

Pros:

- Good data access performance
- No scalability and throughput issue
- Good level of fault tolerance
- Data remains consistent
- Data ownership is preserved

Cons:

- Cloud and containerised configuration hard
- Not good for high volume data
- Not good for high update rates
- Initial service startup dependency

**Data Domain Pattern**

Similar to one discussed in previous chapter for joint ownership.

Pros:

- Good data access performance
- No scalability and throughput issue
- No fault tolerance issues
- Data remains consistent
- No service dependency

Cons:

- Broader bounded context
- Data ownership governance
- Data access security

---

### Chapter 11. Managing Distributed Workflow

Chapter discusses how coordination can take place in a distributed architecture.

**Orchestration Communication Style**

Use an orchestrator (mediator) to manage workflows. Does't include any domain behaviour outside the workflow it manages. Micro service have orchestrator per workflow. Must also model non sunny path cases.

Pros:

- Centralised workflows
- Error handling
- Recoverability
- State management

Cons:

- Responsiveness
- Fault tolerance
- Scalability
- Service coupling

**Choreography Communication Style**

No mediator, service communicates with each other. Introduces semantic coupling. There will always be semantic coupling, and it cannot be reduced. Three different workflow state management pattern:

- Front controller pattern
First service has responsibility of maintaining the state. Some domain service must communicate with the service to update the state.


    Pros:

    - Creates pseudo-orchestrator within choreography
    - Makes querying the state of order trivial

    Cons:

    - Adds additional workflow state to a domain service
    - Increase communication overhead
    - Detrimental to performance and scale as it increases integration communication chatter
- Stateless choreography
No one maintaining state, but this increase cross service communication. Knowing state becomes difficult


    Pros:

    - High performance and scale
    - Extremely decoupled

    Cons:

    - Workflow state must be built on the fly
    - Complexity rises swiftly with complex workflow
- Stamp coupling
Pass on the state information as a request parameter to next service. Would reduce some communication overhead.


    Pros:

    - Allows domain service to pass state without additional query
    - Eliminates need for front controller

    Cons:

    - Larger contracts
    - Doesn't provide just in time status

Trade offs for choreography pattern

Pros:

- Responsiveness
- Stability
- Fault tolerance
- Service decoupling

Cons:

- Distributed workflow
- State management
- Error handling
- Recoverability

**Trade-offs between Orchestration and Choreography**

State owner and coupling. If workflows are easy and need high responsiveness, choreography is the way to go.

---

### Chapter 12. Transactional Sagas

Different patterns discussed are based on the constraints of communication, consistency and coordination mode.

| Patterns | Communication | Consistency | Coordination |
| --- | --- | --- | --- |
| Epic Saga | Synchronous | Atomic | Orchestrate |
| Phone Tag Saga | Synchronous | Atomic  | Choreographed |
| Fairy Tale Saga | Synchronous | Eventual | Orchestrated |
| Time Travel Saga | Synchronous | Eventual | Choreographed |
| Fantasy Fiction Saga | Asynchronous | Atomic | Orchestrated |
| Horror Story Saga | Asynchronous | Atomic | Choreographed |
| Parallel Saga | Asynchronous | Eventual | Orchestrated |
| Anthology Saga | Asynchronous | Eventual | Choreographed |

| Patterns | Coupling | Complexity | Responsiveness | Scale |
| --- | --- | --- | --- | --- |
| Epic Saga | Very high | Low | Low | Very low |
| Phone Tag Saga | High | High | Low | Low |
| Fairy Tale Saga | High | Very low | Medium | High |
| Time Travel Saga | Medium | Low | Medium | High |
| Fantasy Fiction Saga | High | High | Low | Low |
| Horror Story Saga | Medium | Very High | Low | Medium |
| Parallel Saga | Low | Low | High | High |
| Anthology Saga | Very low | High  | High | Very High |

> A pattern is recognition of only commonality and not solvability

---

### Chapter 13. Contracts

This chapter discusses how services communicate and the trade-offs around contract design.

**Strict contracts** are tightly defined contracts like schemas with exact field definitions. Good for clarity and safety but create tight coupling between producer and consumer. Changes require coordination across teams.

**Loose contracts** are flexible contracts where consumers only parse what they need, like name-value pairs or JSON without strict schema. More decoupled but harder to govern and debug. Consumers may silently break if data shape changes.

Strict contracts favour safety and clarity at the cost of coupling and change control. Loose contracts favour decoupling and flexibility at the cost of governance and debugging. The right choice depends on whether your priority is stability or evolvability.

**Consumer-driven contracts** let consumers define what they need from a producer. The producer tests against these expectations. Good middle ground, gives producers freedom to evolve while guaranteeing consumers get what they expect. Prefer consumer-driven contracts when you have many consumers with different needs.

---

### Chapter 14. Managing Analytical Data

This chapter covers how to handle analytical and reporting data in distributed architectures.

**The data warehouse** is the traditional approach of pulling data from all services into a centralised warehouse. Works but creates a monolithic data layer that defeats the purpose of service independence. ETL pipelines become fragile and hard to maintain.

**The data lake** is a more flexible alternative that stores raw data and transforms on read. Better for large volumes and diverse data formats but can become a data swamp without governance.

**The data mesh** treats analytical data as a product owned by domain teams. Each domain publishes its data as a well defined data product with clear ownership, discoverability and quality guarantees. Four principles of data mesh:
- Domain ownership - The team that creates the data owns its analytical data
- Data as a product - Treat data consumers as customers with SLAs and quality standards
- Self-serve data platform - Infrastructure that makes it easy for domains to publish and consume data products
- Federated computational governance - Global standards with local autonomy

Data mesh aligns well with microservice architectures, same principle of domain ownership applied to data. Analytical data is often an afterthought in decomposition, plan for it early.

---

### Chapter 15. Build your own Trade-Off Analysis

The final chapter ties everything together, how to build your own trade-off analysis framework.

**Qualitative analysis** involves listing trade-offs for each option and evaluating them against your architectural drivers. Not everything can be quantified, sometimes a qualitative comparison table is enough to make a decision.

**Quantitative analysis (MAAT)** stands for Modelling Architecture and Technology Trade-offs. Assign weights to architectural characteristics based on priority and score each option against them. Gives a more objective comparison when stakeholders disagree.

Steps for trade-off analysis:
1. Identify the decision - What architectural problem are you solving?
2. List the options - What are the realistic alternatives?
3. Identify trade-offs - For each option what do you gain and what do you lose?
4. Evaluate against drivers - Which trade-offs align best with your architectural drivers and business goals?
5. Document the decision - Use ADRs to capture the context, options and rationale

Every architectural decision is a trade-off. If an option looks perfect you have missed something. Architecture decisions should be recorded and revisitable, they are not permanent.

</details>
