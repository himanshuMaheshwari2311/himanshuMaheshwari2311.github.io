---
title: "The Mythical Man-Month"
date: 2023-06-18
description: "Fred Brooks' classic on the challenges of managing large software projects and why adding manpower to a late project makes it later"
tags: ["software-engineering", "project-management", "books"]
ShowToc: true
---

**Author:** Fred Brooks

**Genre:** Software Engineering, Technology

**Rating:** 4/5

## 🚀 The Book in 3 Sentences

1. How system programming used to happen and the challenges around it
2. Different processes, myths and ways to tackle these problems
3. Principles of software engineering

## 🎨 Impressions

It's not a conventional book for learning about SDLC, but the system programming fanned out will make you think and correlate it with current software engineering practices. This thought exercise was far more valuable to me.

### How I Discovered It

I discovered it on Reddit's must-read book list for software developers.

### Who Should Read It?

Not a must read, but if you are looking for a book on SDLC this can be a good pick.

## ☘️ How the Book Changed Me

- It made me understand why some of the decisions being taken at higher level are in closed meetings — to avoid the input of many and keep everyone on track
- Trying new flashy things is not the way to go, sticking to conceptual integrity of the project is the key

## ✍️ Quotes

> The only constancy is change.

> Skepticism is not pessimism.

> Hardest part of building a software is deciding precisely what to build.

<details>
<summary><strong>📒 Summary + Notes</strong></summary>

*The book is heavily based on system programming (assembly/hardware design/instruction set programming) hence some things would not directly correlate with current software development practices.*

---

### Chapter 1. The Tar Pit

Time taken to develop a standalone application differs heavily from when we need to create a programming system (e.g. a library for a language) or a programming product (e.g. Google Sheets). The most effort is needed to build a programming system product.

**Joys of the craft:**

- Joy of making things
- Making things that are useful to other people
- Fascination of creating complex puzzle-like objects of interlocking and moving parts and watching them work
- Joy of always learning
- Delight of working in a tractable (easy to change) environment

**Woes of the craft:**

- Product managers decide the projects or influence the objectives
- Designing grand concepts is fun, finding bugs is work
- Debugging has linear convergence, testing drags on
- Work when near completion may seem obsolete once it's done

---

### Chapter 2. The Mythical Man-Month

Some reasons why projects lack calendar time:

- Our techniques for estimating are poorly developed
- Estimating techniques fallaciously confuse effort with progress
- Because we are uncertain with estimates, managers are not patient and expect delivery within those estimates
- Schedule progress is poorly monitored
- When schedule slippage is recognised, the response is to add more manpower

All programmers are optimists and this affects our estimates. Men and months are interchangeable only when a task can be partitioned among workers that requires no communication.

The common miss in an estimate is the estimate of communication. Adding more people might not give significant speed since it increases communication overhead.

Typical time distribution on a task:

- 1/3 planning
- 1/6 coding
- 1/4 component test and early integration test
- 1/4 integration test and regression test

---

### Chapter 3. The Surgical Team

Problem with small focused teams is that they're too slow for really big systems.

Conceptual integrity is important — don't introduce radical ideas in the middle of a project. It should be designed by only a few people and shared as RFCs.

**Team structure (Mill's proposal):**

- **Surgeon** — Solution architect (senior/staff engineer)
- **Copilot** — Junior or mid engineer
- **Administrator** — Engineering manager
- **Editor** — RFC reviewer
- **Two secretaries** — Product manager
- **Program clerk** — Technical writer
- **Toolsmith** — Infra/foundation person
- **Tester** — SDET
- **Language lawyer** — Someone extremely proficient in the implementation language

---

### Chapter 4. Aristocracy, Democracy and System Design

Conceptual integrity is the most important consideration in system design. The system should be simple and straightforward to use.

Ideal need for a project is an aristocracy-based team organisation where only few minds take the most important decisions. Even if there are amazing ideas from others, don't compromise the conceptual integrity.

A project's creative effort involves three phases — architecture, implementation, realisation.

---

### Chapter 5. The Second System Effect

Usually the first system an architect builds is very good, since it's their first time and they are meticulous with all details. The second system can have over-design creeping in using ideas that were sidetracked in the first system.

**Interactive discipline for architects when an estimate is too high:**

- Remember that the builder has control over implementation — suggest, not dictate
- Be prepared to suggest a way of implementing anything specified, and accept other ways to meet the objective
- Deal privately and quietly in such suggestions
- Be ready to forego credit for suggested improvements

**Avoiding second system effect:**

- Exert extra self-discipline to avoid functional ornamentation
- Make estimates and boundaries (e.g. capability X is not worth more than M bytes of memory)
- Take review from another architect who has experience building more than two systems

---

### Chapter 6. Passing the Word

How to effectively communicate changes and new proposals (mainly around RFCs).

**Written specification — the Manual:** Should have input from the entire team, but should only be written by one or two people to maintain consistency.

**Conferences and Court:** Two levels of meetings:

- A weekly half-day conference conducted by the chief system architect. Proposals should be shared in writing before the meeting. Success depends on:
    - Same group meeting regularly
    - Group being well-versed with problems, tasks and product
    - Written proposals giving clarity

**The Telephone Log:** The architect maintains a log of every question asked and answered — a resource for the whole team.

---

### Chapter 7. Why Did the Tower of Babel Fail

Even after having a clear mission, manpower, resources, time and technology — a product might fail if communication is not effective. Communication in large projects takes three forms: **informally**, **meetings**, and **workbook**.

**The Project Workbook:** A formal document capturing all project details. Small notes that turn into documentation, helpful for controlling information distribution.

**Organisation:** Teams should be structured so that communication is reduced to a minimum. Authority structure should be tree-like — no one person answering to multiple people.

---

### Chapter 8. Calling the Shot

How does one estimate:

- Don't estimate solely on the coding portion — weigh in the coding portion and apply the ratios
- Time to build isolated small programs is not applicable to large systems
- Effort grows as a power of size (not linearly, even with no communication)

---

### Chapter 9. Ten Pounds in a Five-Pound Sack

More towards system programming — how to deal with trade-offs of memory and space budget for implementation. Can be related to system design trade-offs.

---

### Chapter 10. The Documentary Hypothesis

Documents for a software product should cover:

- Objective
- Specification
- Schedule
- Budget
- Org chart
- Space allocation (can relate to infrastructure)
- Estimate, forecast and prices

Written decisions are essential for clarity. Documents help identify gaps and inconsistencies in thought, implementation, and feasibility.

---

### Chapter 11. Plan to Throw One Away

Any new project with a new implementation — it is highly likely the initial design will be flawed and there will be a redesign. "The only constancy is change."

**Plan the system for change** — always quantize the change.

**Plan the organisation for change** — reluctance to document design comes from reluctance to commit to defending decisions. Managerial structure needs to change as the system changes (Conway's Law).

**Two steps forward and one step back** — once shipped, maintenance begins. Total cost of maintaining is usually 40% more than the cost of developing. With time more domain cases are explored as users increase.

**One step forward and one step back** — total modules increase linearly with release number, but affected modules increase exponentially. (This may not hold if microservices are truly decoupled.)

---

### Chapter 12. Sharp Tools

How to set up environments for the development lifecycle. Analogous to having staging + production environments and testing environments.

---

### Chapter 13. The Whole and The Parts

Testing and debugging of a system.

**Designing the bugs out:**

- Bug-proofing the definition — describe the task in detail, make sure everyone is aligned
- Testing the specification — test all possible scenarios, edge cases and wrong cases
- Top-down design — design a high-level test suite, then drill down on individual functions
- Structured programming — make sure the design is simple and readable

**System Debugging:** Control changes as you merge fixes. Add one component at a time. Quantize updates.

---

### Chapter 14. Hatching a Catastrophe

Even single-day delays can accumulate and cause uncertainty.

**Milestones:**

1. Estimates revised carefully every two weeks before the activity starts
2. Overestimates come down as activity proceeds
3. Underestimates don't change significantly until about three weeks before scheduled completion

**Under the Rug:** Developers hide slips from managers, managers hide from their managers. Two techniques:

1. **Reducing role conflict** — distinguish between action information and status information. Boss must not act on problems his manager can solve
2. **Yanking off the rug** — stringent review meetings, stick to the roadmap and timelines

---

### Chapter 15. The Other Face

Documentation for different audiences:

1. **To use a program** — prose description covering: purpose, environment, domain and range, functions and algorithms, I/O formats, operating instructions, options, running time, accuracy
2. **To believe a program** — ship with sample use cases: mainline case, barely legitimate case, barely illegitimate case
3. **To modify a program** — include flow charts or diagrams so others understand how to modify the code

**Self-Documenting programs** — similar to Swagger, where code is annotated and a utility generates documentation. More maintainable than separate files.

---

### Chapter 16. No Silver Bullet — Essence and Accident

There is no single system that can radically change the software development process. Four essential difficulties:

1. **Complexity** — software is complex for its size compared to any other human construct. Poor communication leads to product flaws, cost overrun and schedule delays
2. **Conformity** — underlying foundations and concepts can change at any time
3. **Changeability** — software can be changed anytime, challenging conceptual integrity
4. **Invisibility** — software is not tangible, making it difficult for people to agree on specifications

**Past breakthroughs that solved accidental difficulties:** high-level languages, time sharing, unified programming environments.

**Promising attacks on conceptual essence:**

1. Buy vs build
2. Requirements refinement and rapid prototyping
3. Incremental development — grow, not build software
4. Great designers — develop ways to grow great designers

---

### Chapter 17. No Silver Bullet — Refired

Most complexities are symptoms of organisation malfunctions.

**Proposed silver bullets:**

1. Harel's — incorporate diagrams to explain systems and processes (combat invisibility)
2. Focus on quality and productivity will follow
3. Object-oriented programming
4. Reusing software components — buying vs building

</details>
