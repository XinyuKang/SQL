# Knowledge about computer networking

## Communication Protocals
### User Datagram Protocol (UDP/IP)
The User Datagram Protocol, or UDP, is a communication protocol used across the Internet for especially **time-sensitive** transmissions such as video playback or DNS lookups. It speeds up communications by **not formally establishing a connection before data is transferred**. This allows data to be transferred very quickly, but it can also cause packets to become lost in transit.

### TCP vs. UDP
![tcpUdp](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/df37487a-b934-4dd9-9853-c7b577953f77)

UDP is faster but less reliable than TCP, another common transport protocol. In a TCP communication, the two computers begin by establishing a connection via an automated process called a **handshake**. Only once this handshake has been completed will one computer actually transfer data packets to the other.

UDP communications do not go through this process. Instead, one computer can simply begin sending data to the other.

In addition, TCP communications indicate the order in which data packets should be received and confirm that packets arrive as intended. If a packet does not arrive — e.g. due to congestion in intermediary networks — TCP requires that it be re-sent. **UDP communications do not include any of this functionality (no order or arrival check)**.

These differences create some advantages. Because UDP does not require a ‘handshake’ or check whether data arrives properly, it is able to transfer data much faster than TCP.

However, this speed creates tradeoffs. If a UDP datagram is lost in transit, it will not be re-sent. As a result, applications that use UDP must be able to tolerate errors, loss, and duplication.

## Different Casts (transferring of data)
![cast](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/f4dbdefd-4167-447b-b7d9-951132878804)

### Multicast
Multicast is group communication where data transmission is addressed to a group of destination computers simultaneously.

Unicast uses TCP for communications while multicast communication uses UDP.


## Multithread Application
### Synchronization
Is the coordination of multiple threads to control their access to any shared resource. Makes sure only **one thread can access the resource at a given point in time**.
![mutexAsemaphore](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/52e7c807-9244-4ec6-bfbf-c1c69669be3c)

#### [mutex](https://www.geeksforgeeks.org/mutex-vs-semaphore/): an object
Stands for **mutual exclusion object** which provides mutual exclusion to a specific portion of the code (the `ciritcal section`) where no two processes can exist in the critical section at any given point of time.
- If after entering into the critical section, the thread sleeps or gets preempted by a high priority process, no other thread can enter into the critical section. This can lead to starvation.
- Implementation of mutex can lead to busy waiting that leads to the wastage of the CPU cycle.

#### [Semaphore](https://www.geeksforgeeks.org/mutex-vs-semaphore/): an integer
A semaphore is a **non-negative integer** variable that is shared between various threads. Semaphore uses two atomic operations for process synchronization: `Wait (P)` and `Signal (V)`. 
- Only one process will access the critical section at a time, however, multiple threads are allowed.

#### mutex vs semaphore
A mutex is a locking mechanism used to synchronize access to a resource. Only one task (can be a thread or process based on OS abstraction) can acquire the mutex. It means there is ownership associated with a mutex, and **only the owner can release the lock (mutex)**. 

**A binary semaphore is NOT protecting a resource from access. A binary semaphore can be signaled by any thread (or process).**. 

Mutex:

Is a key to a toilet. One person can have the key - occupy the toilet - at the time. When finished, the person gives (frees) the key to the next person in the queue.

Officially: "Mutexes are typically used to serialise access to a section of re-entrant code that cannot be executed concurrently by more than one thread. A mutex object only allows one thread into a controlled section, forcing other threads which attempt to gain access to that section to wait until the first thread has exited from that section." Ref: Symbian Developer Library

(A mutex is really a semaphore with value 1.)

Semaphore:

Is the number of free identical toilet keys. Example, say we have four toilets with identical locks and keys. The semaphore count - the count of keys - is set to 4 at beginning (all four toilets are free), then the count value is decremented as people are coming in. If all toilets are full, ie. there are no free keys left, the semaphore count is 0. Now, when eq. one person leaves the toilet, semaphore is increased to 1 (one free key), and given to the next person in the queue.

Officially: "A semaphore restricts the number of simultaneous users of a shared resource up to a maximum number. Threads can request access to the resource (decrementing the semaphore), and can signal that they have finished using the resource (incrementing the semaphore)." Ref: Symbian Developer Library

### Confition Variables
A condition variable is a data structure associated with a condition; it allows threads to block until the condition becomes true. For example, thread_pop might want check whether the queue is empty and, if so, wait for a condition like “queue not empty”.
