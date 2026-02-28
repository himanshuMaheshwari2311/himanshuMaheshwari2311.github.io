---
title: "The Clean Coder"
date: 2024-02-25
description: "Robert C. Martin's guide to professionalism, discipline, and ethics for software developers"
tags: ["software-engineering", "professionalism", "books"]
ShowToc: true
---

**Author:** Robert C. Martin

**Genre:** Software Engineering, Technology

**Rating:** Lifechanging

## 🚀 The Book in 3 Sentences

1. The book is a guide for software developers seeking to elevate their professionalism and ethics in coding
2. It emphasises the importance of writing clean, maintainable code while advocating for personal responsibility, effective communication, and continuous improvement in one's skill
3. Through anecdotes, practical advice, and real-world examples, the book empowers developers to uphold high standards of professionalism and integrity in their work

## 🎨 Impressions

The book is a manual that every developer should live by and in the process strive to find a discipline that works the best for them. It clearly states what is a developer's responsibility. You are responsible for the goals of the organization. It is a developer's responsibility to up-skill themselves and stay sharp.

It gives good tricks on how to become a better developer by practicing, time management, making commitments and when to say no. It places quality at the centre of delivering software and teaches TDD and various testing strategies.

### How I Discovered It

As part of mentorship program, a recommendation from Misha.

### Who Should Read It?

Every developer should read it once.

## ☘️ How the Book Changed Me

- I am better aware about my responsibility
- I will try to practice soft skills from chapters — saying yes, saying no and time management
- QA should not find any bugs is what I will strive for when working on a feature and writing code
- I feel I have a better understanding of the impact that bad work can have compared to staying up to standards while working
- I will incorporate TDD in the features I work on

## ✍️ Quotes

> Don't follow your passion, rather let it follow you in your quest to become.

<details>
<summary><strong>📒 Summary + Notes</strong></summary>

### Chapter 1. Professionalism

The chapter talks about habits and disciplines of a software professional and what it takes to be a professional.

Being a professional is about taking **responsibility** and **accountability** for the work you do. How does a professional take responsibility:

- **Do no harm to function**
    - We must not create bugs. Sounds impossible but by keeping ourselves responsible for the imperfections and apologising and learning from this would mean taking responsibility
    - QA should find nothing. It is unprofessional to send code to QA which we know is faulty or which we are uncertain about
    - You must know it works — the only way is by having 100% test coverage
- **Do no harm to structure**
    - We must always leave the code better than what it was before when we touch a module
    - If you want software to be flexible, you have to do it

**Work Ethics** are an integral part to being professional. At work you are responsible for performing and in your free time you are responsible to be better at the profession. You can be a better professional by:

- **Know your field** — You should know all the tools, principles and patterns available to solve a problem
- **Continuous learning** — No one would want a professional who is outdated
- **Practice** — To keep your skills sharp you must practice. Kata's are one form of practicing
- **Collaboration** — You can learn a lot from other people, like pair programming
- **Mentoring** — By teaching a topic you also keep questioning the concepts
- **Know your domain** — Not just the technical part, but you should know the domain you are working on
- **Identify with your customer/employer** — The problems of your customer are your problems
- **Humility** — Everyone makes mistakes, by being humble you accept those shortcomings

---

### Chapter 2. Saying No

Professionals speak truth to power, they have the courage to say no to their managers.

- We need an **adversarial role** within the team or while making a decision, someone who can confront when making hard decisions
- The most important time to say no is when **stakes are high**
- Being a **team player** is all about positioning yourself well and helping out teammates when they get into a jam
- There is **no trying.** Promising to try is fundamentally dishonest and you are probably doing it to save face or avoid confrontation
- Sometimes the only right yes, is being unafraid to say no
- A professional must always keep in mind that he doesn't have to be a hero. Doing the job well on time following a discipline is the right way

---

### Chapter 3. Saying Yes

When saying yes, it is a commitment that you are making.

- By saying yes, you cannot be uncertain about it. You have to **make a commitment** — you say you'll do it, you mean it, and you actually do it
- You can recognise **lack of commitment** if there are words like need, should, hope, wish, etc.
- A **commitment** is an assertive statement. When in doubt, think about things you can have control over and make a commitment on it
    - If you rely on someone else to reach an end goal, commit to specific actions that bring you closer to the goal
    - If you can't make a commitment, the most important job is to raise a red flag to the manager
- You can learn how to say yes by drawing a line about what is achievable and communicating it clearly without compromising the disciplines

---

### Chapter 4. Coding

This chapter talks about soft skills, discipline and pitfalls to avoid when coding.

- Coding is a mentally challenging exercise that needs concentration. You need to be **prepared** to write good code that:
    - Works and solves the problem
    - Solves the customer's problem only, meets their needs
    - Fits well into existing system — be flexible, robust and transparent
    - Is readable and maintainable
- When you have **other topics on your mind**, clear them out first and then get back to coding
- Falling into the **flow zone** might give a sense of working faster, but often you have tunnel vision. Avoid it by pairing with someone
- **Interruptions** can distract your train of thought. When someone interrupts, ask them to wait if you are busy, or help them
- When you **can't write code**, take a break, pair with someone
- **Creative input** like reading sci-fi can help you be more creative while writing code
- Time spent **debugging** can be avoided with well-written tests and coverage — the goal should always be zero debugging time
- Coding is a marathon, not a race. **Pace yourself** — know when you are stuck and need to walk away
- If you can't **finish code on time**, detect early and be transparent. Don't factor in hope — it is the project killer. Don't rush — code written in haste is waste
- When you need to work **overtime**, know your limits
- Always define the **definition of done** by sharing acceptance tests with stakeholders
- You'll need **help** while coding — it is unprofessional to remain stuck when help is available

---

### Chapter 5. Test Driven Development

Three laws of TDD:

1. Don't write code until you have a failing test
2. Write a test that is small enough to fail
3. Only write the code needed to pass the test

Benefits of TDD:

- **Certainty** about the behaviour of the code
- **Defects** would rarely be introduced
- **Courage** to mercilessly refactor knowing tests cover intended functionality
- Serves as **documentation** of system behaviour
- Helps you **design** code that is decoupled and has smaller logical units

---

### Chapter 6. Practicing

Practicing skills in a group in the form of coding dojo's. Different forms:

- **Kata's** — Problems you practice alone, simulating all professional disciplines
- **Wasa** — Two-person kata: one writes unit tests while the other passes them, then swap
- **Randori** — N people participate, taking turns writing tests and passing them in a circle

Also: open source contribution and practicing ethics in your own time.

---

### Chapter 7. Acceptance Tests

Acceptance tests keep clear communication between stakeholder and developer.

**Premature precision** is a trap when trying to finalise requirements. This leads to discussions trying to perfect requirements which waste time.

- **Uncertainty principle** — Things appear different on paper and on systems. Build small, show customers, get feedback
- **Estimation anxiety** — Always include error bars when estimating on low precision

Solution is to defer precision as late as possible. Developers can use acceptance tests to avoid pitfalls:

- **Definition of done** — everything is done: code, deployment, QA, and acceptance by stakeholders
- All acceptance tests should be **automated**
- Stakeholders seldom have time to write acceptance tests, so developers should write them along with QA
- Negotiate for better tests when they are vague

---

### Chapter 8. Testing Strategies

QA should be collaborators, not adversaries. They play two roles:

- **Specifiers** — acceptance tests serve as system specification
- **Characterisers** — characterise system behaviour with specifications

The test automation pyramid:

- **Unit tests** (100% coverage) — low-level system specification
- **Component tests** (50%) — acceptance tests, testing logical components in isolation
- **Integration tests** (20%) — how well components interact (choreography tests)
- **System tests** (10%) — throughput, performance, testing system construction
- **Exploratory tests** (5%) — manual tests

---

### Chapter 9. Time Management

**Meetings** are expensive but necessary. Use time wisely:

- Ask yourself if you are needed; politely decline if not
- When meetings derail, leave politely or bring everyone back to agenda
- Have a goal and agenda with time slots
- If disagreement doesn't settle in five minutes, you need more data

**Focus** is crucial to high-quality work. Improve focus by:

- Sleep well
- Manage caffeine intake
- Recharge by walking or doing activities
- Muscle focus to shift from mental drain
- Have a source of inspiration

**Time boxing** — 25-minute focused blocks with 5-minute breaks for interruptions.

**Avoiding tasks** — priority inversion is when you find something else to do instead of the uncomfortable task. Recognise when you are wrong and work to fix it.

---

### Chapter 10. Estimation

Business views estimates as commitments, but they are rough guesses to developers.

- **Commitment** is about certainty — don't commit to what you can't achieve
- **Estimate** is a distribution: best case, average case, worst case
- Beware of **implied commitment** — "I'll try" may implicitly mean committing

**PERT (Trivariate analysis)** — uses optimistic, nominal, and pessimistic estimates.

**Wideband Delphi** — group estimation: flying fingers, planning poker, affinity estimation.

**Law of large numbers** — break big tasks into smaller ones and re-estimate. The combined estimate will be larger than the original big-task estimate.

---

### Chapter 11. Pressure

Best way to stay calm under pressure is to **avoid the situations** that cause it:

- Don't accept **commitments** you know you can't meet
- **Stay clean** — quick and dirty always slows you down
- Stay true to your **discipline** — if you abandon it in crisis, it doesn't help

When pressure is unavoidable:

- **Don't panic** — think it through, then make a plan
- **Communicate** thoughts with team and manager, avoid surprises
- **Rely on your discipline** — code as you would normally
- **Get help** — pair with someone, and help others under pressure

---

### Chapter 12. Collaboration

**Programmer vs employer** — Keep an eye on the goals of the organization. That is your first responsibility.

**Programmer vs programmer** — You work fast and learn more when you collaborate. Don't build walls around your code — anyone in the team should be able to work on any module. Pairing helps since you're not just working but also learning.

---

### Chapter 13. Teams and Projects

Teams should not be built around projects. A gelled team can work much more effectively — they anticipate each other and work well together. It takes time to form such a team.

Gelled teams can easily switch between projects. Teams built around projects cannot do that.

---

### Chapter 14. Mentorship, Apprenticeship and Craftsmanship

**Mentor** — With a good mentor you can learn new things quickly. As a mentor you can guide someone and relearn in the process. An unconventional way of mentoring is to learn by observing others.

**Apprenticeship** — A necessity for young developers to grow. We should focus on imparting technical skills and discipline.

**Craftsmanship** — All about skill, quality, experience, and competence. You make others follow the discipline by following it yourself and setting an example.

</details>
