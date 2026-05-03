# Assignment 3 - Complete Documentation

**Student Name**: [lama ali alahmari]  
**Student ID**: [445052030]  
**Date Submitted**: [3 may ]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [may 2, 2026, 7:00 PM]
**What I implemented**: 
I began by reading the assignment guidelines and comprehending the CPU scheduler code that was supplied. I found common resources like executionLog, totalWaitingTime, contextSwitchCount, and completedProcessCount.
**Challenges encountered**: 
At first, it was difficult to understand where race conditions could appear in the code.
**How I solved it**: 
I carefully examined common variables accessed by several threads and went over synchronisation ideas from the textbook.
**Testing approach**: 
Before implementing synchronisation, I ran the application once to see how it behaved.
**Time spent**: 
2 hours
---

### Entry 2 - [may 3, 2026, 2:00 PM]
**What I implemented**: 
I put ReentrantLock in place to safeguard executionLog and shared counters. I included try-finally blocks in lock.lock() and lock.unlock().
**Challenges encountered**: 
ensuring that, in the event of an exception, locks are always released.

How I figured it out:
**How I solved it**: 
To ensure that unlock() is always called, I utilised try-finally blocks.
**Testing approach**: 
I tested each procedure separately to make sure there were no execution errors.
**Time spent**: 
1 hours
---

### Entry 3 - [may 3, 2026, 8:00 PM]
**What I implemented**: 
To manage CPU access and make sure that only one process is active at a time, I implemented a semaphore.
**Challenges encountered**: 
knowing the proper locations for acquire() and release().
**How I solved it**: 
I included release() inside finally blocks and acquire() at the beginning of execution.
**Testing approach**: 
To ensure consistent execution behaviour, I ran the application several times.
**Time spent**: 
3hours
---

### Entry 4 - [may 3, 2026, 9:00 PM]
**What I implemented**: 
I checked the system as a whole and confirmed that the synchronisation was correct.
**Challenges encountered**: 
maintaining consistency in the output throughout several runs.
**How I solved it**: 
I compared the results after running the software five times.
**Testing approach**: 
manual result checking and repeated execution.
**Time spent**: 
1 hours
---

### Entry 5 - [may 3, 2026, 10:00 PM]
**What I implemented**: 
final evaluation and writing of the documentation.
**Challenges encountered**: 
clearly summarising all efforts in the documentation.
**How I solved it**: 
I went over my code and wrote step-by-step explanations.
**Testing approach**: 
Make one last check to make sure there are no mistakes before submitting.
**Time spent**: 
1 hours
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[The shared variable contextSwitchCount has one race situation where several threads simultaneously increase the value. Increments may be missed in the absence of synchronisation, resulting in inaccurate counts. An ArrayList called executionLog, which is accessed by several threads, has another race issue. Runtime problems like ConcurrentModificationException or inconsistent logs can result from concurrent edits. These problems arise when threads run concurrently without mutual exclusion, which causes erratic behaviour.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Only one thread can access a crucial portion at a time thanks to reentrant lock, which ensures mutual exclusion. It is employed in this assignment to safeguard shared variables, such as executionLog and counters. Conversely, a semaphore is used to manage several threads' access to a resource. Here, only one process can use the CPU at a time thanks to a binary semaphore (1 permit). Semaphores regulate resource access, whereas locks safeguard data]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[When two or more threads wait endlessly for resources owned by one another, this is known as deadlock. Using try-finally blocks to guarantee that locks are always released is one preventative strategy. Reducing the number of locks and preventing circular wait situations are two other strategies. I avoided deadlocks in this assignment by utilising a single lock for shared resources and always releasing locks and semaphores in finally blocks.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[To safeguard all three counters, I employed a single lock (coarse-grained locking). By doing this, deadlocks that might arise from using multiple locks are prevented and the architecture is made simpler. Reduced concurrency is the trade-off because only one thread can update any counter at once. By enabling many threads to update various counters at the same time, fine-grained locking would increase concurrency. It does, however, make things more complicated and error-prone. Coarse-grained locking is suitable since the assignment emphasises accuracy and simplicity. Fine-grained locking might improve concurrency because the counters are independent, but it is not required in this case.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables
Which variables: 
totalWaitingTime, completedProcessCount, and contextSwitchCount
Why they need protection: 
They can be changed concurrently and are shared by several threads.
Synchronization mechanism used:
Reentrant Lock
Code snippet:
// Paste your implementation here
```lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}

**Justification**: 
prevents inaccurate numbers by ensuring that only one thread updates counters at a time.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
Concurrent access can result in exceptions, as ArrayList is not thread-safe.
**Synchronization mechanism used**: 
Reentrant Lock
**Code snippet**:
```java
// Paste your implementation here
```lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}

**Justification**: 
provides consistent logging and stops concurrent modification.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Limit access to the CPU's execution
**Number of permits and why**: 
One permission will only permit one process at a time.
**Where implemented**: 
In runToCompletion() and run()
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
    try {
    // execution
      } finally {
    SharedResources.cpuSemaphore.release();
}
// Paste your implementation here

Effect on program behavior: 
avoids simultaneous CPU utilisation and guarantees sequential execution.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
**Results**: 
Total Context Switches: 23
Total Completed Processes: 11
Total Waiting Time: 584777ms
Average Waiting Time: 53161ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         2            7858         39278       
P2         1            6178         43160       
P3         5            9273         64392       
P4         2            8566         65674       
P5         5            7460         53379       
P6         5            5894         56848       
P7         1            8119         66249       
P8         4            6483         62767       
P9         1            3317         32216       
P10        4            3686         35549       
P11        3            7116         65265

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
Without synchronisation, race circumstances could lead to inconsistent logs and inaccurate tallies.
**Conclusion**: 
Synchronisation guarantees accurate and consistent outcomes.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
ran the program several times while recording its execution.There was no 
**Results**: 
There was no ConcurrentModificationException.
**What this proves**: 
executionLog is adequately safeguarded.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
Completed processes = total processes
Context switches > 0
Waiting time > 0
**Actual values**: 
Matched expected behavior in all runs.
**Analysis**: 
The program behaves correctly under synchronization.
---

### Test 4: Different Scenarios
**Scenario tested**: [Different process numbers and burst times]

**Purpose**: 
Test robustness
**Results**: 
Program handled all scenarios correctly
**What I learned**: 
Synchronization ensures stability across conditions
---

## Part 5: Reflection and Learning
### What I learned about synchronization:

[I discovered how race problems arise in multithreaded systems and how to use locks and semaphores to prevent them. I recognised the significance of limiting access to important areas and safeguarding shared resources. I also discovered that inadequate synchronisation can result in inaccurate outcomes and unstable systems.]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

Example 1: Banking systems in which the same account balance is updated by several transactions

Example 2: Scheduling of the operating system when several processes share CPU resources

---

### How I would explain synchronization to others:

[Similar to a queue, synchronisation allows just one individual to utilise a resource at a time. Semaphores regulate the number of threads that can access a resource, while locks guarantee that only one thread can enter a crucial area.]

---
## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/lamaahmari/OS-Assignment3-lama-alahmari.git
**Number of commits**: 
18
**Commit messages**: 
1.Set my student ID
2.Add synchronization libraries
3.Initialize lock and semaphore
4.Protect shared counters
5.Protect execution log
6.Implement semaphore


---

## Summary

**Total time spent on assignment**:
9–10 hours
**Key takeaways**: 
1.When multithreading, synchronisation is crucial.
2.Race situations are avoided via locks.
3.Resource access is managed by semaphores.

**Most challenging aspect**:
Recognising racial conditions and using locks correctly
**What I'm most proud of**:
Implementing synchronisation successfully and getting consistent outcomes
---

**End of Documentation**
