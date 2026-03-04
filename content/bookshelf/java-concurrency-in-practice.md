---
title: "Java Concurrency In Practice"
date: 2024-04-06
description: "Brian Goetz's definitive guide to writing correct, performant and maintainable concurrent programs in Java"
tags: ["software-engineering", "concurrency", "java", "books"]
ShowToc: true
---

**Author:** Brian Goetz

**Genre:** Software Engineering, Technology

**Rating:** 5/5

## 🚀 The Book in 3 Sentences

1. The book is a guide to writing thread-safe concurrent programs in Java, teaching you how to reason about shared mutable state
2. It covers everything from the fundamentals of thread safety and visibility to building concurrent data structures, task execution frameworks and the Java memory model
3. It emphasises making code correct first and fast second, with practical patterns for composing thread-safe objects and avoiding concurrency hazards

## 🎨 Impressions

The book is dense but every chapter builds on the previous one in a way that makes concurrency feel approachable. It changed how I think about shared mutable state — it is at the core of every concurrency bug. The patterns around immutability, confinement and safe publication are things I now instinctively reach for. Even though some Java APIs have evolved, the principles in this book are timeless.

### How I Discovered It

It is one of the most recommended books for Java developers on every must-read list. I picked it up while working with multithreaded systems at work.

### Who Should Read It?

Every Java developer should read it once. Even if you use frameworks that abstract away threads, understanding how concurrency works under the hood makes you a better developer.

## ☘️ How the Book Changed Me

- I think about thread safety as managing access to shared mutable state, not just putting `synchronized` everywhere
- I will reach for immutability and confinement first before reaching for locks
- I am more aware of visibility issues and how stale data can cause subtle bugs that are hard to reproduce
- I feel I have a better mental model of the Java memory model and happens-before relationships
- I will focus on making code correct first and only optimise synchronisation when there is a proven bottleneck

## ✍️ Quotes

> It is far easier to design a class to be thread-safe than to retrofit it for thread safety later.

> The problem with threads is not that they are hard to use — it is that they are easy to use incorrectly.

> Always focus on making your code right first and then make it fast.

<details>
<summary><strong>📒 Summary + Notes</strong></summary>

### Chapter 1. Introduction

**History of concurrency**

Multi processing was introduced to have better resource utilisation, fairness of resource allocation and convenience to run programs faster. Now threads are treated as basic unit of scheduling and not processes

**Benefits of threads**

- Exploiting multiple processors - when multithreaded programs are designed properly they can improve the throughput of the program by utilising the processor effectively. They can also help improve performance on single thread systems when they are IO bound.
- Simplicity of modelling - It is easy to break down a complex async workflow into simpler sync workflows running on separate thread and interacting with each other through synchronisation points
- Simplified handling of async events - with time the support for threads in OS has improved far significantly compared to async IO

**Risks of thread**

Using threads is a double edged swords if you don't know how to work with them.

- Safety hazards - access to objects by multiple threads can leave the program in an non deterministic state
- Liveness hazards - Liveness concerns that something good eventually happens. Deadlock, starvation and livelock can cause program to be unresponsive permanently
- Performance hazards - More threads doesn't necessarily mean more throughput linearly. Using synchronisation can inhibit compiler optimisations, flush or invalidate memory cache

---

### Chapter 2. Thread Safety

Writing thread safe code is at it's core managing access to state and in particular shared mutable state. Locks and threads are just means to make this happen. An objects state encompasses any data that can affect it's external visibility. To make object thread safe question how the object will be used rather than focusing on what it does

Way to fix a broken multi threaded program with mutable object is

- Don't share state of variable across threads
- Make state variable immutable
- Use synchronisation when accessing state of variable

When designing thread safe classes, good object oriented techniques - encapsulation, immutability and clear specification of invariants (conditions in program) are your best friend

Always focus on making your code right first and then make it fast

**What is thread safety?**

Definition: A class is thread safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment and with no additional synchronisation or other coordination on the part of the calling code

Local variables are stored on the stack of the thread executing  the code, hence it is not accessed by any other thread. A stateless object is thread safe

**Atomicity**

Atomicity means same as it does in transactional application like RDBMS, the group of statement that appear together execute as single, indivisible unit. A code that might look simple might not be atomic. A simple increment is not atomic, it is a read-modify-write operation and in multithreaded program this can cause undesirable state because of unlucky timings, it is called race conditions.

- Race conditions - it occurs when correctness of computation depends on relative timing. In contrast to read-modify-write, we can also have check-then-act which can be caused by a stale observation by a thread
- Compound actions - To ensure thread safety check-then-act and read-modify-write operations should be atomic.

**Locking**

Lock is used to ensure only one thread accesses the block/ variable at a time. When there are multiple state variables to preserve state consistency, update related state variables in a single atomic operation.

- Intrinsic lock - Every java object can act as a lock for purpose of synchronisation and hence it is called an intrinsic lock or monitor lock
- Reentrancy - Intrinsic locks are reentrant. If a thread tries to acquire a lock it already holds it succeeds. Reentrancy means locks are acquired per thread basis not per invocation. It is good for object oriented design

**Guarding state with locks**

Locks can be used to provide a guarantee of exclusive access to a shared state. To ensure this all the access to a mutable shared variable should be done by the same lock.

Make it clear to maintainers what are the locks associated with the shared state

For every invariant that involves more than one variable, all the variables involved in that invariant must be guarded by the same lock.

**Liveness and performance**

Blocking the entire multithreaded code with sync blocks is not performant. It gets rid of compiler optimisation as well. Make sure to identify dependent operations and invariants and lock only the smallest part needed

Always try to implement the synchronisation policy as simply as possible rather than chasing performance from the get go.

A small tip would be to avoid holding locks during IO operation or lengthy computations

---

### Chapter 3: Sharing Objects

This chapter aims at techniques for sharing and publishing objects so that they can be accessed safely by multi threaded programs

**Visibility**

Visibility is very counterintuitive when working in a multithreaded program, we never know what order the operations are executed by compiler, processor and runtime when there is no synchronisation, which can lead to threads seeing data which can be stale or have intermediate state not intended for that thread.

- Stale data - It is analogous to read uncommitted in database, a thread can read old data for a state when other thread is making update to that value. This can cause serious safety and liveness failure
- Non atomic 64 bit operation - Java treats 64 bit long and double as two 32 bit reads which was written in mind to support old architecture. This is still true till date.
- Locking and visibility - Locking helps in memory visibility too. To ensure all threads see the most up to date value of shared mutable variable, reading and writing thread must synchronise on common lock
- Volatile variables - Volatile is a weaker form of synchronisation. When a variable is marked as volatile the compiler and runtime puts a notice that the variable is shared and operation on it should not be reordered with other memory operation.
Don't use volatile variable for visibility of the state. Good example can be to ensure visibility of their own state, or object that they refer to or use as flags
Locking guarantees both visibility and atomicity, volatile only guarantees visibility
Use volatile variable when the following criteria are met:
    - Writes are independent of current value, or you can ensure writes are from single thread only
    - Variable does not participate in invariants with other state variable
    - Locking is not required for any other reason while variable is being accessed

**Publication and escape**

Publishing means making an object available outside it's scope, like passing it to a method or returning it from a non-private method. We ideally want to share a object but we must do it in a thread safe manner. A unsafe publication means an object have escaped.

Publishing an object also publishes any objects that are referred by its non private fields. Once an object escapes you have to assume that other class/ thread may carelessly misuse it. Inner class publishes reference to its enclosing instance

- Safe construction practices - Do not allow this reference to escape during construction it can lead to publishing an incompletely constructed object. Do not start a thread in a constructor (inner class). Calling an override-able instance method from constructor (calling parent class method) can leak this reference. All these can be avoided by creating a private constructor and a public factory method

**Thread confinement**

Accessing shared mutable state requires synchronisation and one way to avoid it is to not share this state. Thread confinement would ensure the data is accessed only from a single thread. There are no language construct to confine an object to a thread.

- Ad-hoc thread confinement - This is when maintaining thread confinement falls entirely on the implementation.
- Stack confinement - Local scope variables only exists on the threads stack, so these would always be thread confined. Primitive types can never escape thread confinement but objects are responsibility of developer to make it stack confined. Make sure the local variable does not escape
- ThreadLocal - Global variables can be easily shared as thread locals. This ensures each thread has a copy of the global variable.

**Immutability**

Immutable objects are always thread safe. An object is immutable if, it's state cannot be modified after construction, all fields are final and it is properly constructed (no this reference escaping). Immutable object offer performance advantage of reduced need for locking or defensive copies and reduced impact on generational garbage collector, with a little cost of allocation

- Final fields - An object in which we can make a few fields final can reduce the possible state complexity and is mostly immutable. As marking a field final is a good practice if we don't need greater visibility. Marking field as final is a good practice unless we need to make it mutable.

**Safe publication**

- Improper publication - You cannot rely on integrity of partially constructed objects, initialisation of objects is not safe. A thread could see stale value for the object itself, and if the object is not properly constructed it sees a stale value for the state of the object.
- Immutable object and initialisation safety - Java memory model guarantees initialisation safety for immutable object. All fields that are final are constructed properly. If there are non final fields JVM initialises object with default value (base constructor) for non final fields and then assigns the value passed to the constructor
- Safe publication idioms - For mutable objects to be published safely, both reference to the object and the objects state must be made visible to each other thread at the same time. A **properly constructed object** can be safely published by
    - Initialising object reference from a static initialiser, they are executed by JVM at class initialisation time because of internal sync in the JVM. This guarantees safe publication
    - Storing reference in a volatile fields or an AtomicReference
    - Storing reference in a final field of properly constructed object
    - Storing reference in a field that is properly guarded by a lock
- Effectively immutable objects - Object that are not mutable but whose state cannot be modified after publication are called effectively immutable. Such objects are safe to publish
- Mutable objects - Published mutable objects need synchronisation for read and modification to state. Publication requirement of object depends on the mutability
    - Immutable objects can be published through any mechanisms
    - Effectively immutable objects must be safely published
    - Mutable objects must be safely published and must be either thread safe or locked
- Sharing objects safely - When you publish an object you document how it should be accessed

The most useful policies for using and sharing objects in a concurrent program are:

- Thread-confined. A thread-confined object is owned exclusively by and confined to one thread, and can be modified by its owning thread.
- Shared read-only. A shared read-only object can be accessed concurrently by multiple threads without additional synchronisation, but cannot be modified by any thread. Shared read-only objects include immutable and effectively immutable objects.
- Shared thread-safe. A thread-safe object performs synchronisation internally, so multiple threads can freely access it through its public interface without further synchronisation.
- Guarded. A guarded object can be accessed only with a specific lock held. Guarded objects include those that are encapsulated within other thread-safe objects and published objects that are known to be guarded by a specific lock.

---

### Chapter 4: Composing Objects

**Designing a thread-safe class**

Designing thread safe class has three basic elements

1. Identify the variables that form the state
2. Identify the invariants that constraint the state variable
3. Establish a policy for managing concurrent access to object state

A sync policy specifies how an object coordinates its state without violating it's invariants or post conditions. It specifies what combination of immutability, thread confinement and locking is used to maintain a thread state

- Gathering synchronisation requirements - To ensure thread safety you need to understand the invariants and post condition of the object. Multivariable invariants can lead to atomicity requirements
- State dependent operations - Operations with state based preconditions are called state dependent. Some tasks might wait for a pre condition to become true to process it further, this can be done with wait notify or it's much easier to use blocking queues
- State ownership - An object should own the state it encapsulates and provide locking mechanism to access the state. GC makes us think less about ownership since reference sharing is easy. If we return a reference the ownership becomes shared. Collections have split ownership, hence which working with collections the objects stored should be either thread safe, immutable, effectively immutable or guarded by lock

**Instance confinement**

It is easy to control all the code paths when a mutable object is encapsulated in a class. This along with providing access to the non thread safe object with a locking mechanism can provide thread safe access to the object. These confined object should not escape their intended scope

- The java monitor pattern - Java monitor pattern is instance confinement. It encapsulates all the mutable state and guards it with object's **intrinsic lock**. Always ensure the locks are private else the client code using the class encapsulating the state can participate in synchronisation.

**Delegating thread safety**

When composing an object with other thread safe object, it depends if we need another layer of thread safety

- Independent state variable - Delegating thread safety to a single state variable is no problem. It is not a problem again even with multiple state variables when there is no invariant tying them together. They should be independent
- When delegation does not fail - If a class if composed of multiple independent thread safe state variable and has no operation that have any invalid state transition, then it can delegate thread safety to underlying state variable
- Publishing underlying state variables - If a state variable is thread-safe, does not participate in any invariants that constrain its value, and has no prohibited state transitions for any of its operations, then it can safely be published

**Adding functionality to existing thread-safe class**

Safest way is to modify the original class, to do so you need to understand the underlying synchronisation policy so you can follow the original design.

Another way is extending the class and adding code to the subclass, but this can cause the synchronisation to be distributed in multiple classes.

- Client side locking - In order to use client side locking you must know which lock to use. Client side locking is fragile since it puts locking code for a class in a totally unrelated class. It violates the encapsulation of the sync policy
- Composition - Adding new features by composing is less fragile. You can wrap the class instance and implement locking on the new feature.

**Documenting synchronisation policies**

Document a class's thread safety guarantees for its client and its sync policy for its maintainers.

- Interpreting vague documentation - When there are no documentation about the thread safety of a class to use it you will have to make a guess. Understand the context in which the object is being used and then make the assumption. Try to understand the code from a perspective of implementing it rather than using it

---

### Chapter 5. Building blocks

Delegating is one of the most effective strategy to create thread safe class.

**Synchronised collections**

The synchronised collection in java wraps the underlying collection and makes every public method sync, so only one thread can access the state at a time

- Problem with synchronised collections - These collections commits to client side locking which can cause issue if we don't know which lock to use.
- Iterators and concurrent modification exception - Iterators of sync collection are not designed to deal with concurrent modification, they are fail fast iterators. If we modify a collection when other thread is iterating over it we get a ConcurrentModificationException. This is done without sync block. Alternative to avoid is to clone the collection since it will be thread confined
- Hidden iterators - Some operations although might not look like an iteration can have hidden iterators. A string concatenation of a collection would cause an iteration to start. Greater the distance between a state and sync that guards it you'd likely forget to use proper sync. Encapsulating an objects state makes it easy to preserve it's invariants and encapsulating it's synchronisation makes it easier to enforce sync policy

**Concurrent collections**

Unlike synchronised collection which serialises access to underlying collection, concurrent collections are built for concurrent access from multiple threads. These collection does not throw concurrent modification exception, since they are weakly consistent instead of fail fast

- Concurrent hash maps - Concurrent hash maps have lock striping that provide fine grained locking mechanism. Methods such as size and isEmpty are weakened to give better performance. It does not provide exclusive access just to a single thread
- Copy on write array list - These collection rely on the fact that effectively immutable objects can give thread safety.

**Blocking queue and producer consumer pattern**

Blocking queues block (timed wait) put when the queue is full and block take when the queue is empty, this can be leveraged to easily implement a producer consumer pattern. Thread pools use this queue to incorporate this pattern.

- Serial thread confinement - For handing off variables from one thread to another blocking queue are a great candidates. Object pools leverage this to hand off tasks to threads
- Deque and work stealing - Deque can be used for work stealing. As opposed to all consumers having one queue in producer consumer pattern, each consumer have their own deque in work stealing pattern. If a consumer has completed all the task in its deque it can take a task from other consumers deque tail

**Blocking and interruptible methods**

Blocking is basically waiting for IO, acquire a lock or waiting for other thread to complete execution. Interruption is a cooperative mechanism that lets on thread interrupt other thread. Most common use case is to cancel activity. Blocking methods should be interruptible

**Synchronisers**

Objects that can coordinate control flow of threads based on its state are synchronisers. Blocking queue is an example of synchroniser. Synchroniser share a certain structural property, they encapsulate state that determine if the thread arriving at the synchroniser should be allowed to pass or not.

- Latches - Latch delays the progress of thread until it reaches a terminal state. It acts as a gate hence it can be only changed once. It can be used to ensure certain activities do no proceed until other one-time condition is me. Some examples are
    - Resource initialisation, do not initialise the service until dependent service has started
    - Multiplayer game lobby only proceed once all players are ready
- Future tasks - It also acts like a latch. Once a future task enters a completed state it stays in that state forever. It can be used to start some early computation whose result can be accessed later on
- Semaphores - Counting semaphores are used to control the number of activities that can access a certain resource or perform a given action at same time. Can be used in resource pool or to impose a bound to a collection.
- Barriers - Barriers are similar to latches with a difference that barriers wait for threads to arrive and latches wait for events to arrive. Can be used to wait for computation from sub problems of a parallel iterative algorithm.
Once all threads reach a barrier, a unique arrival index is assigned to each thread which can be used to elect a leader that takes some special action in next iteration. One form of barrier is cyclic barrier. Another form is exchanger which is useful to exchange information between threads

---

### Chapter 6. Task execution

Most concurrent applications are organised around the execution of tasks: abstract, discrete units of work. Dividing the work of an application into tasks simplifies program organization, facilitates error recovery by providing natural transaction boundaries, and promotes concurrency by providing a natural structure for parallelising work.

**Executing tasks in threads**

- Executing tasks sequentially - This can lead to app being unresponsive under load, the cpu remains idle if there are heavy IO operations
- Explicitly creating threads for task - With this the main thread gets requests and offloads it to another thread, this has 3 requirement/effect
    - Off load task processing from main thread
    - Task can be processed in parallel
    - Task handling code must be thread safe
- Disadvantage of unbounded thread creation - Disadvantages are
    - Thread lifecycle overhead. Creation and teardown of thread are expensive
    - Resource consumption
    - Stability, there is limit to the threads that can be created

**The executor framework**

Executor framework is a thread pool implementation by provided by java. It decouples task submission from task executions (tasks are written as Runnable). It also provides lifecycle support and hooks for statistics, app management and monitoring

- Execution policies - It defines, what, where, when and how of task execution. Policies are resource management tool and optimal policy depends on resources and QoS requirement. Separating execution policy from task submission allows us to change the configuration in different environment independently
- Thread pools - Thread pools are homogeneous pool of worker thread tightly bound to work queue. Advantage of thread pool is avoid thread lifecycle overhead for executing task frequently
- Executor lifecycle - In order for JVM to shutdown non daemon thread should terminate, hence failing to shut down thread pool can prevent JVM from exiting. Executor lifecycle has three state - running, shutdown and terminated.
Task submitted to executor service after shutdown are handled by rejected execution handler. It is common to follow shutdown with awaitTermination
- Delayed and periodic tasks - Use scheduled thread pools over timer task as they provide relative time execution. To implement your own scheduling service, you can use DelayQueue

**Finding exploitable parallelism**

- Result bearing tasks - Callable and Future support can return value which Runnable cannot. Lifecycle of task executed by executor has 4 phase - created, submitted, started and completed. Future encapsulates the return value for callable and provides method around lifecycle of the task.
- Limitation of parallelising heterogeneous tasks - Heterogeneous task cannot be executed in the same thread pool as they require different configuration. Some task may take more time to execute compared to other which won't give the best performance.
- Completion service - This acts as a handle for batch computation the same way future acts for single computation.
- Placing time limits on tasks - If some activity does not complete within a given time, the result may no longer be needed. It will also in turn save resource.

---

### Chapter 7. Cancellation and shutdown

Java does not provide a mechanism to safely force a thread to stop, hence cancelling a task becomes tricky. Java provides interruption, which is a cooperative mechanism

**Task cancellation**

An activity is cancellable if external code can move it to completion before it's normal completion. Reason for cancellation can be user triggered cancellation, time limited activities, application event, errors, shutdown. There is no safe way to preempt a task/thread in java hence we need cooperative mechanisms

- Interruption - Interruption is a mechanism to cancel a thread which can be initiated from another thread hence cooperative. JVM makes no guarantees on how quick a blocking method with detect interruption, but it is reasonably quick.
Calling interrupt doesn't mean target thread has stopped, it just means that interruption request has been delivered.
- Interruption policies - Interruption policy determines how a thread interrupts a cancellation request. Ideal thread-level/service-level cancellation should exit as quickly, cleaning up resource/ completing the processing and notifying any parent entity that it is exiting.
When dealing with interruption for threads belonging to for example thread pools, make sure to restore the interrupted status after performing the actions you need to on interrupt so the parent entity can deal with the interrupt as well.
- Responding to interruption - Practical way to respond to interrupts are
    - Propagate the exception after clean up, making the method interruptible and blocking
    - Restore interrupt status so higher parent code can deal with the interrupt request

    Only code implementing the interruption policy should consume the interruption request. Other code should always restore the state after acting on it. Interruptible methods polls to check the interrupt status. Choosing the polling frequency is a trade-off between efficiency and responsiveness

- Cancellation via future - Future.cancel(true) can interrupt a running task. This is the preferred way to cancel tasks submitted to an executor. Always check the return value — cancellation is best-effort, not guaranteed
- Dealing with non-interruptible blocking - Not all blocking operations respond to interruption. Socket I/O, synchronous I/O and intrinsic lock acquisition are not interruptible. For these, you must override the thread's interrupt behaviour — e.g. closing the underlying socket to unblock the thread
- Encapsulating non-standard cancellation with newTaskFor - By overriding newTaskFor in ThreadPoolExecutor, you can return a custom Future that encapsulates non-standard cancellation logic, keeping the cancellation mechanism hidden from the caller

**Stopping a thread based service**

Applications commonly create services that own threads (e.g. logging services, thread pools). These services should provide lifecycle methods so they can be shut down cleanly.

- Executor service shutdown - ExecutorService provides shutdown() for graceful shutdown (finish running tasks, reject new ones) and shutdownNow() for abrupt shutdown (attempt to cancel running tasks, return unstarted ones). Choose based on whether you need to drain the queue
- Poison pills - A special recognisable object placed on the work queue that means "stop processing." Works well when the number of producers and consumers is known. Producers send poison pills and consumers stop when they receive one
- Limitations of shutdownNow - shutdownNow returns tasks that were submitted but never started, but there is no general way to find out which tasks were in progress at shutdown. You need custom tracking if you need to know what was interrupted

**Handling abnormal thread termination**

A thread that terminates due to an uncaught exception can silently disappear, leading to mysterious failures. Always handle exceptions in thread pool tasks.

- Uncaught exception handlers - Thread.UncaughtExceptionHandler lets you detect when a thread dies due to an uncaught exception. For thread pools using execute(), the handler is invoked; for submit(), the exception is wrapped in the Future. At minimum, log the exception in the handler

**JVM shutdown**

The JVM can shut down in an orderly fashion (last non-daemon thread terminates, System.exit) or abruptly (Runtime.halt, kill signal).

- Shutdown hooks - Registered via Runtime.addShutdownHook, these run concurrently during orderly shutdown. Use them for cleanup like deleting temp files or flushing logs. Keep them short and thread-safe since they run in parallel with other shutdown hooks
- Daemon threads - Daemon threads are terminated abruptly when the JVM shuts down — their finally blocks are not executed. Use them only for housekeeping tasks, not for anything requiring cleanup
- Finalisers - Avoid finalisers. They offer no guarantees about when (or if) they run, and they add significant overhead to garbage collection. Use try-with-resources or explicit close methods instead

---

### Chapter 8. Applying thread pools

**Implicit coupling between task and execution policies**

Some tasks have implicit requirements on the execution policy that can cause problems if not matched correctly.

- Thread starvation deadlock - If tasks that are submitted to a thread pool depend on other tasks in the same pool, they can deadlock. A single-threaded executor will deadlock if a task submits another task and waits for its result. In bounded pools, tasks that depend on other tasks in the same pool risk starvation deadlock
- Long running tasks - Tasks that take a long time can clog the pool and make it unresponsive to other tasks. Use timed versions of blocking methods (e.g. timed poll instead of take) so long-running tasks don't monopolise threads indefinitely

**Sizing thread pools**

Thread pool size depends on the nature of the tasks. CPU-bound tasks benefit from N+1 threads (where N is number of CPUs). I/O-bound tasks need larger pools since threads spend time waiting. The formula accounts for the ratio of wait time to compute time: N * U * (1 + W/C), where U is target CPU utilisation and W/C is the ratio of wait to compute time.

**Configuring thread pool executors**

- Thread creation and teardown - Core pool size is the target size; threads are created lazily up to this size. Maximum pool size is the upper bound. Threads beyond the core size are reaped after the keep-alive time expires
- Managing queued tasks - Three basic approaches: unbounded queue (LinkedBlockingQueue), bounded queue (ArrayBlockingQueue), and synchronous handoff (SynchronousQueue). Bounded queues prevent resource exhaustion but require a saturation policy
- Saturation policies - When a bounded queue fills up, the saturation policy determines what happens. AbortPolicy throws an exception, CallerRunsPolicy runs the task in the submitting thread (provides natural back-pressure), DiscardPolicy silently drops the task, DiscardOldestPolicy drops the oldest queued task
- Thread factories - Custom thread factories let you set thread names, UncaughtExceptionHandlers, daemon status, and priority. Naming threads aids debugging enormously
- Customising thread pool executor after construction - Most configuration can be changed after construction via setters. If you don't want this, wrap the executor with Executors.unconfigurableExecutorService

**Extending thread pool executor**

ThreadPoolExecutor provides beforeExecute, afterExecute, and terminated hooks. These can be used to add logging, timing, monitoring, or statistics gathering. afterExecute is called whether the task completes normally or throws an exception (but not if it throws an Error).

**Parallelising recursive algorithms**

Sequential loops where iterations are independent are good candidates for parallelisation. Recursive algorithms where each iteration may spawn more work can be parallelised by submitting each recursive step as a task. Use invokeAll or a CompletionService to collect results. This pattern works well for tree traversals, puzzle solvers, and divide-and-conquer algorithms.

---

### Chapter 9. GUI Applications

**Why are GUI single-threaded**

Almost all GUI frameworks are single-threaded, using an event dispatch thread (EDT). Multi-threaded GUI frameworks have historically failed due to the complexity of ensuring consistency between input event processing and the visual representation.

- Sequential event processing - GUI events are processed sequentially on the EDT. This means long-running tasks on the EDT freeze the UI
- Thread confinement in Swing - Swing components and models are confined to the EDT. Any access to GUI objects must happen on the EDT via SwingUtilities.invokeLater or invokeAndWait

**Short-running GUI tasks**

Short tasks can run entirely on the EDT without affecting responsiveness. The pattern: user action triggers event on EDT, handler updates the model and view, all on the EDT.

**Long-running GUI tasks**

Long tasks must be offloaded from the EDT to a background thread to keep the UI responsive.

- Cancellation - Use Future.cancel to cancel long-running background tasks. The GUI should provide a cancel button that triggers cancellation and updates the UI on the EDT
- Progress and completion indication - Background tasks should report progress back to the EDT using invokeLater. SwingWorker provides built-in support for progress reporting and completion callbacks

**Shared data models**

When background threads need to access the data model, thread safety must be addressed.

- Thread safe data models - The model itself can be made thread-safe using synchronisation, but this can cause inconsistent views if the model is read without holding the lock throughout
- Split data models - Use a presentation model (EDT-confined) and a shared model (thread-safe). Background threads update the shared model, which periodically syncs with the presentation model on the EDT

**Other forms of single-threaded subsystems**

The pattern of confining work to a single thread is not unique to GUIs. Any subsystem that is not thread-safe can be accessed safely by confining it to a single thread and using a work queue pattern.

---

### Chapter 10. Avoiding liveness hazards

**Deadlock**

Deadlock occurs when two or more threads are each waiting for a lock held by the other. The dining philosophers problem is the classic illustration.

- Lock-ordering deadlocks - If all threads acquire locks in the same order, lock-ordering deadlocks cannot occur. Define a global ordering on locks and always acquire them in that order
- Dynamic lock order deadlocks - When lock order depends on runtime arguments (e.g. transferring money between two accounts), use a tie-breaking mechanism like System.identityHashCode to impose a consistent order
- Deadlocks between cooperating objects - Acquiring one lock and then calling an alien method that acquires another lock is a recipe for deadlock. The call chain may not be obvious if methods call into other objects while holding locks
- Open calls - A call to a method with no locks held is an open call. Restructure code to use open calls — gather data under the lock, then call external methods after releasing it. This greatly reduces deadlock risk
- Resource deadlocks - Deadlock can also occur when threads wait for resources (e.g. database connections) in different orders, or when a single-threaded executor task submits another task and waits for its result

**Avoiding and diagnosing deadlocks**

- Timed lock attempts - Use tryLock with a timeout instead of unconditional locking. If the lock isn't acquired in time, back off, log a failure, and retry. This prevents permanent deadlock at the cost of livelock risk
- Deadlock analysis with thread dumps - JVM thread dumps show which locks each thread holds and is waiting for. The JVM can even identify deadlock cycles in its thread dump output. Use kill -3, jstack, or JConsole to get thread dumps

**Other liveness hazards**

- Starvation - A thread is perpetually denied access to resources it needs (e.g. CPU time). Avoid using thread priorities as they are platform-dependent and can cause starvation
- Poor responsiveness - If a thread holds a lock for a long time, other threads waiting for that lock appear unresponsive. Keep lock hold times short
- Livelock - A thread is not blocked but cannot make progress because it keeps retrying an operation that always fails (e.g. two threads repeatedly backing off and retrying in sync). Introduce randomness in retry timing to break the cycle

---

### Chapter 11. Performance and scalability

**Thinking about performance**

- Performance vs scalability - Performance is "how fast" for a single unit of work; scalability is "how much" throughput with more resources. They sometimes conflict — optimising for single-thread performance can hurt scalability
- Evaluating performance tradeoffs - Avoid premature optimisation. Most performance decisions involve tradeoffs. Measure first, then optimise the bottleneck. "Make it right, then make it fast"

**Amdahl's Law**

The theoretical speedup from parallelisation is limited by the fraction of work that must be done sequentially. If F is the serial fraction, speedup on N processors is at most 1/(F + (1-F)/N). Even small serial fractions limit scalability significantly. All concurrent applications have some serialisation — the key is to minimise it.

**Costs introduced by threads**

- Context switching - Switching between threads incurs overhead from saving/restoring thread state, cache misses, and scheduler overhead. Frequent context switching (e.g. from excessive locking) hurts throughput
- Memory synchronisation - Synchronisation instructions (memory barriers) inhibit compiler optimisations like instruction reordering and can force cache flushes. Uncontended synchronisation is cheap on modern JVMs, but contended locks are expensive
- Blocking - When a thread blocks on a contended lock, the JVM can either spin-wait or suspend the thread. Suspension involves two context switches and OS-level operations

**Reducing lock contention**

The biggest threat to scalability is exclusive resource locks. Three ways to reduce contention: hold locks for shorter durations, request locks less frequently, or use alternative coordination mechanisms.

- Narrowing lock scope - "Get in, get out." Move code that doesn't need the lock outside the synchronized block. Don't do I/O or expensive computations while holding a lock
- Reducing lock granularity - Use different locks to guard independent state variables instead of one lock for everything. This allows greater concurrency
- Lock striping - A form of lock granularity where a variable number of locks are used to guard different parts of a data structure (e.g. ConcurrentHashMap uses 16 locks for different hash buckets)
- Avoiding hot fields - Shared fields accessed frequently by many threads (e.g. a counter) become serialisation bottlenecks. Techniques like ConcurrentHashMap weakening size() avoid this
- Alternative to exclusive locks - ReadWriteLock allows concurrent reads. Atomic variables use CAS operations to avoid locking entirely for single-variable state
- Monitoring CPU utilisation - If CPUs are not fully utilised, the causes are usually: insufficient load, I/O-bound workload, lock contention, or external bottleneck. Identify which and address it
- Just saying no to object pooling - Object pooling was useful when allocation was expensive, but modern JVMs allocate faster than most pools can synchronise. Pooling introduces contention and is rarely beneficial for lightweight objects

**Reducing context switch overhead**

Log processing is a good example: instead of logging in the request-processing thread (with locks), use a producer-consumer pattern with a dedicated logging thread. This consolidates I/O and reduces context switches.

---

### Chapter 12. Testing concurrent programs

Testing concurrent programs is fundamentally harder than testing sequential ones because bugs may only manifest under specific timing conditions. Tests must verify both safety (nothing bad happens) and liveness (something good eventually happens).

Key testing strategies:
- **Testing for correctness** — Use test cases that exercise concurrent access patterns. Create multiple threads performing operations simultaneously and verify the result is consistent
- **Testing for performance** — Measure throughput, responsiveness, and scalability under varying thread counts and workloads
- **Avoiding pitfalls** — Ensure tests can actually fail when there's a bug. Use barriers (CyclicBarrier) to start threads simultaneously. Use countdown latches for coordination. Include enough operations to trigger interleaving
- **Generating interleavings** — Use Thread.yield() and short sleeps strategically to increase the chance of exposing timing-dependent bugs. Tools like FindBugs can detect common concurrency errors statically

---

### Chapter 13. Explicit locks

**ReentrantLock** provides the same mutual exclusion and memory visibility as synchronized, but with additional features:

- **Timed and polled lock acquisition** — tryLock avoids blocking forever and allows deadlock recovery
- **Interruptible lock acquisition** — lockInterruptibly allows a thread to be interrupted while waiting for a lock
- **Fairness** — Fair locks grant access in FIFO order, preventing starvation but reducing throughput due to the overhead of suspending and resuming threads. Non-fair locks permit barging (a thread that requests the lock at the right moment can skip the queue)
- **Performance** — ReentrantLock significantly outperformed synchronized in Java 5, but Java 6 closed the gap. Use synchronized unless you need the advanced features

**ReadWriteLock** allows multiple concurrent readers or a single writer. It improves scalability when reads greatly outnumber writes. The implementation must decide on fairness, reentrancy, and upgrade/downgrade policies.

Choose synchronized by default. Use ReentrantLock when you need timed/polled/interruptible locking or fair queuing.

---

### Chapter 14. Building custom synchronisers

**Managing state dependence** — State-dependent operations must block until preconditions are met. The standard approach uses condition queues.

- **Condition queues** — Every Java object can act as a condition queue via wait(), notify(), and notifyAll(). A thread calls wait() to release the lock and block until notified. Always test the condition in a while loop (spurious wakeups)
- **Explicit Condition objects** — java.util.concurrent.locks.Condition provides multiple wait sets per lock, timed waits, and fairness control. More expressive than intrinsic condition queues
- **AbstractQueuedSynchronizer (AQS)** — The framework behind ReentrantLock, Semaphore, CountDownLatch, FutureTask, and ReentrantReadWriteLock. AQS manages an integer state and a FIFO queue of waiting threads. Custom synchronisers extend AQS and implement tryAcquire/tryRelease

---

### Chapter 15. Atomic variables and non-blocking synchronisation

**Compare-and-swap (CAS)** is the foundation of non-blocking synchronisation. It atomically updates a variable only if it has the expected value, otherwise it fails and retries. CAS is a hardware instruction supported by modern processors.

- **Atomic variable classes** — AtomicInteger, AtomicLong, AtomicReference, etc. provide lock-free thread-safe operations using CAS. They offer better scalability than locks under moderate to high contention because they don't cause thread suspension
- **Non-blocking algorithms** — Algorithms that use CAS instead of locks. A thread's failure or suspension does not prevent other threads from making progress. Examples include non-blocking stacks, queues (like ConcurrentLinkedQueue), and counters
- **The ABA problem** — CAS can be fooled if a value changes from A to B and back to A. AtomicStampedReference solves this by pairing the reference with a version stamp

Non-blocking algorithms are significantly harder to design and implement but provide better liveness guarantees (no deadlock) and often better throughput.

---

### Chapter 16. Java memory model

The Java Memory Model (JMM) defines when changes made by one thread become visible to another. It specifies a partial ordering called **happens-before** that determines memory visibility.

- **Happens-before rules** — Key rules include: program order within a thread, monitor lock release happens-before subsequent acquire of the same lock, volatile write happens-before subsequent read, thread start happens-before any action in the started thread, thread termination happens-before join returning
- **Publication and initialisation safety** — The JMM guarantees that immutable objects (all final fields, proper construction) are safely published without synchronisation. For mutable objects, you need explicit synchronisation or safe publication idioms
- **Double-checked locking** — The infamous broken pattern for lazy initialisation. It doesn't work without volatile because the JMM allows the compiler/processor to reorder the object's construction and reference assignment. Use volatile or the lazy initialisation holder class idiom instead

The JMM is what makes the rules about synchronisation, volatile, and final fields work. Understanding it helps reason about when synchronisation is truly needed.

</details>
