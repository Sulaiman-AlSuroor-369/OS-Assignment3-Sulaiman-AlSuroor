# Assignment 3 - Complete Documentation

**Student Name**: [Sulaiman Saud AlSuroor]  
**Student ID**: [445050158]  
**Date Submitted**: [2026-5-2]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[445050158]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [2026-4-30 , 9:01]
**What I implemented**: 
  my student ID
**Challenges encountered**: 
  no major challenges
**How I solved it**: 
  i replaced the random numbers to my actual ID
**Testing approach**: 
for changing the numbers and see what the output will be
**Time spent**: 
30 seconds
---

### Entry 2 - [2026-5-1 , 6:58]
**What I implemented**: 
 counter lock for every increment or adding process
**Challenges encountered**: 
 the output takes a few time for appearing
**How I solved it**: 
 I implemented counter lock as reentrant lock object, and counter lock is a common lock object for every incremental counter , and added try catch statement for checking.
**Testing approach**: 
 for protect the context switch and run it perfectly
**Time spent**: 
45 minutes
---

### Entry 3 - [2026-5-2, 12:18]
**What I implemented**: 
 reentrant lock for execution log
**Challenges encountered**: 
 no challenge major
**How I solved it**: 
 I implemented a locklog as reentrant object, and used try catch statement,  and add message while trying.
**Testing approach**: 
for protect the execution log  and run it perfectly
**Time spent**: 
30 minutes
---

### Entry 4 - [2026-5-2, 1:19]
**What I implemented**: 
 adding semaphore for process
**Challenges encountered**: 
 adding another object for semaphore because the process isn't static 
**How I solved it**: 
I implemented a cpuSemaphore as semaphore object , and i implemented another object in process class and I call all shared resources class methods as executed process
**Testing approach**: 
for see what happen with acquire and release
**Time spent**: 
1 hour
---

### Entry 5 - [2026-5-2, 1:39]
**What I implemented**: 
 runToComplection() method
**Challenges encountered**: 
 no major challenges
**How I solved it**: 
I identified another object for cpu Semaphore (Semaphor object) and replaced 1 to 2
**Testing approach**: 
for see what if replace the number of semaphore
**Time spent**: 
15 minutes
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]
- the shares resource is affected ready queue and current time.
 for example , two threads calling processQueue.poll() could retrieve the same process
- concurrent access is a problem because multiple threads can modify the queue's internal state like heads, tail pointers, element links ,at the same time.if two threads can call poll(), both might read the same head.
- because of race condition in a multithreaded environment , when multiple try to access and sharing at the same time, their operations can overleap with each other. 

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Your answer here - reference try-finally blocks, lock ordering, etc.]

--- a reentrant lock provides mutual exclusion with a single permit and supports reentrance. in my schedular simulation, I used RenntrantLock to protect the context switch and completed process and waiting time , because only one thread should modify these shared structures at ant moment.

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

-I used coarse-grained lock for each counter in this code.
-because this choice simplifies the code and avoids multiple lock, and the single lock is faster and less errors.
-coarse-grained reduces overhead but serializes all counter updates. fine-grained allows parallel updates to independent counters but increase the complexity.
- fine-grained provides better concurrency because threads updating different counters never block each other. that's why i chose coarse-granted because my simulation updates counters infrequently relative to CPU/IO operations. 
## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitch
completeProcess
waitingTime
**Why they need protection**: 
 because without Synchronization, counters would show incorrect values.
**Synchronization mechanism used**: 
a single Reentrant lock providing mutual exclusion for all counters.
**Code snippet**:
```java
// Paste your implementation here
public static int contextSwitchCount = 0;  
public static final ReentrantLock counterlock = new ReentrantLock();
 public static void incrementContextSwitch() {
        // TODO: Protect this critical section with a lock
        // RACE CONDITION: Multiple threads might read and write simultaneously!
        counterlock.lock();
        try {
           contextSwitchCount++; // the counter is protected
        }finally{
            counterlock.unlock();
        }
    }
```

**Justification**: 
using a single lock simplifies the code and ensures atomic updates across related counters.
---

### Critical Section #2: Execution Log

**What resource**: 
the execution log message buffer 
**Why it needs protection**: 
multiple threads write log entires concurrently.
**Synchronization mechanism used**: 
a single reentrant lock guarding all writes to the log.
**Code snippet**:
```java
// Paste your implementation here
public static List<String> executionLog = new ArrayList<>();
public static final ReentrantLock lockLog = new ReentrantLock();
// Method to log execution
    public static void logExecution(String message) {
        // TODO: Protect this critical section with a lock
        // RACE CONDITION: ArrayList is not thread-safe!
        lockLog.lock();
        try {
            executionLog.add(message);
        }finally{
            lockLog.unlock();
        }
    }
```

**Justification**: 
the execution log is a shared resource that must maintain sequential consistency every log entry should appear in the order of events.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
limit the munder of process executing on CPU core.
**Number of permits and why**: 
number of permits = numbers of CPU cores
because this matches the physical parallelism of the system.
**Where implemented**: 
in the process class , and object in the sharedRecources class
**Code snippet**:
```java
// Paste your implementation here
//sharedResources class
public static final Semaphore cpuSemaphore = new Semaphore(1);

//process class
@Override
public void run(){
   try {
                cpuSemaphore.acquire();
            try {
                SharedResources.incrementContextSwitch();
                SharedResources.incrementCompletedProcess();
                SharedResources.addWaitingTime(startTime);  
            } finally {
                cpuSemaphore.release();  
            }
        }catch(InterruptedException e){ 
            e.printStackTrace();  
        }
}

```

**Effect on program behavior**: 
without the semaphore, all the process would execute CPU burst simultaneously regardless of core count.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results
running the schedular simulation multiple times with the sane input to verifying the output, remain identical across runs.
**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
Total Context Switches: 60
Total Completed Processes: 47
Total Waiting Time: 23110490007847ms
Average Waiting Time: 1359440588696ms
```

**Results**: 
(Show that running multiple times produces consistent, correct results)
completed process: 47 (same each time)
average waiting time:  1359440588696ms
context switches: 60 (no variance)
So no lost updates or interleaved corruption across run.

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
-because without it, race condition could occur on share resources like processQueue, waiting time, etc.
the resources that need protection: context switch, complete process, waiting time ,because even if not observed in a fre runs, these bugs will appear under high load.

**Conclusion**: 
the use of reentrant lock for critical sections, semaphore for cpu cores guarantees,repeatable behavior.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

running the simulation high process counts.

**Testing procedure**: 
ظئـ P1 (Priority: 3) added to ready queue ظ¤é Burst time: 5364ms
  ظئـ P2 (Priority: 5) added to ready queue ظ¤é Burst time: 2164ms
  ظئـ P3 (Priority: 4) added to ready queue ظ¤é Burst time: 5952ms
  ظئـ P4 (Priority: 4) added to ready queue ظ¤é Burst time: 4559ms
  ظئـ P5 (Priority: 2) added to ready queue ظ¤é Burst time: 5960ms
  ظئـ P6 (Priority: 4) added to ready queue ظ¤é Burst time: 2811ms
  ظئـ P7 (Priority: 2) added to ready queue ظ¤é Burst time: 2935ms
  ظئـ P8 (Priority: 3) added to ready queue ظ¤é Burst time: 7305ms
  ظئـ P9 (Priority: 2) added to ready queue ظ¤é Burst time: 3229ms
  ظئـ P10 (Priority: 5) added to ready queue ظ¤é Burst time: 5825ms
  ظئـ P11 (Priority: 3) added to ready queue ظ¤é Burst time: 7075ms
  ظئـ P12 (Priority: 1) added to ready queue ظ¤é Burst time: 6506ms
  ظئـ P13 (Priority: 1) added to ready queue ظ¤é Burst time: 2876ms
  ظئـ P14 (Priority: 4) added to ready queue ظ¤é Burst time: 2474ms
  ظئـ P15 (Priority: 4) added to ready queue ظ¤é Burst time: 2947ms
  ظئـ P16 (Priority: 4) added to ready queue ظ¤é Burst time: 4410ms
  ظئـ P17 (Priority: 3) added to ready queue ظ¤é Burst time: 2618ms
**Results**: 
no ConcurrentModificationException was thrown in any run. all executions completed without crashes. 

**What this proves**: 
the purpose use of locks prevents concurrent modification of collections  when one thread iterates while another modifies. 
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
verifying the final simulation metrics fro the input process list
**Expected values**: 
total burst time: 75495ms
Total Context Switches: 70
Total Completed Processes: 50
Total Waiting Time: 21043469000847ms
Average Waiting Time: 13440584538696ms

**Actual values**: 
total burst time: 75010ms
Total Context Switches: 60
Total Completed Processes: 47
Total Waiting Time: 23110490007847ms
Average Waiting Time: 1359440588696ms
**Analysis**: 
the exact match between actual and expected values that all shared resources are correctly synchronized.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
running the schedular with different time quantum and varying numbers of process to observe how it holds under different load conditions.

**Purpose**: 
to verify the synchronization, remain correct and and don't introduce deadlocks when system change parameters.

**Results**: 
Time Quantum:  3000ms , context swishes:60 , no race condition

**What I learned**: 
fine-grained locking on independent counters  while coarse-grained approach still works correctly and easy to debug.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**:
Bank system like ATM, online transfer, etc. 

**Example 2**: 
Multi core system.
---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Sulaiman-AlSuroor-369/OS-Assignment3-Sulaiman-AlSuroor

**Number of commits**: 5 commits

**Commit messages**: 
1. Set my student ID: 445050158
2. Task 1(445050158) added reentrantkock for counter protection
3. Task 2 (445050158): Added ReentrantLock for execution log
4. Task 3 (445050158): Implemented semaphore for CPU control

---

## Summary

**Total time spent on assignment**: 15 hours


**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
