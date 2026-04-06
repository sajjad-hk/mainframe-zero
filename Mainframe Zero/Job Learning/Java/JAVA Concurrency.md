---
tags:
  - Learning
source: https://codesignal.com/learn/courses/introduction-to-java-concurrency
---
## How to create a Thread in Java
### Extending the `Thread` class

### Implementing the `Runnable` interface

## Java Thread Lifecycle
1. NEW: When tread is first create is in NEW state, meaning it is defined but not started yet.
2. RUNNABLE: When start method has been called thread moves to RUNNABLE state.
3. BLOCKED
4. WAITING
5. TIMED_WAITING
6. TERMINATED

### Operations
- `start`
- `join`
- `sleep`
- `setPriority` **Thread priority in Java is not a guarantee of execution order**
### Common Issues with Shared Variables
* Race conditions (Inconsistent result)
* Non-atomic operations (Interrupted operation leads to unpredictable behavior)
* `synchronized`
	* Data integrity
	* Safe execution
	* Real-World significance
	`synchronized` block helps prevent race condition and insure data integrity
### Java Memory Model
Using `volatile` to avoid memory consistency errors
* Heap memory shard by all threads and is where objects are stored.
* Each thread has its own Thread stack which has method calls and local variables. This is private to each thread.
### Double-check blocking pattern
Using `volatile` and `synchronized` block

```Java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {
        // Initialization code
    }

    public static Singleton getInstance() {
        if (instance == null) { // First check without synchronization
            synchronized (Singleton.class) { // Synchronized block
                if (instance == null) { // Second check with synchronization
                    instance = new Singleton(); // Create the instance
                }
            }
        }
        return instance;
    }
}
```

### Type of Locks in Java
* Object Locks `synchronized(this)`
* Class Locks `synchronized(ClassName.class)`
## Deadlock
1. Mutual exclusion when resources are not sharable
2. Hold and wait
3. No preemption when the resource can only be released voluntarily
4. Circular wait
### Solution
1. Acquire locks in consistent order

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    private final Lock lock1 = new ReentrantLock();
    private final Lock lock2 = new ReentrantLock();

    public void methodOne() {
        lock1.lock();
        try {
            System.out.println("Thread 1 acquired lock1");
            try { Thread.sleep(50); } catch (InterruptedException e) {}

            lock2.lock();
            try {
                System.out.println("Thread 1 acquired lock2");
            } finally {
                lock2.unlock();
            }
        } finally {
            lock1.unlock();
        }
    }

    public void methodTwo() {
        lock1.lock(); // Consistent lock order
        try {
            System.out.println("Thread 2 acquired lock1");
            try { Thread.sleep(50); } catch (InterruptedException e) {}

            lock2.lock();
            try {
                System.out.println("Thread 2 acquired lock2");
            } finally {
                lock2.unlock();
            }
        } finally {
            lock1.unlock();
        }
    }
}
```

### Dinning Philosopher 
## Thread Coordination
Using `wait` and `notify`
```Java
class Countdown {

    private boolean isReady = false;

    public synchronized void waitForCountdown() throws InterruptedException {
        while (!isReady) {
            System.out.println("Waiting for countdown...");
            wait(); // Wait until notified
        }
        System.out.println("Countdown complete! Proceeding...");
    }

    public synchronized void startCountdown() throws InterruptedException {
        System.out.println("Countdown started...");
        Thread.sleep(2000); // Simulate countdown time
        isReady = true;
        notify(); // Notify the waiting thread
        System.out.println("Countdown finished! Notified waiting thread.");
    }
}

public class Main {

    public static void main(String[] args) throws InterruptedException {
        Countdown countdown = new Countdown();

        // Thread that waits for the countdown
        Thread waiter = new Thread(() -> {
            try {
                countdown.waitForCountdown();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        // Thread that performs the countdown
        Thread notifier = new Thread(() -> {
            try {
                countdown.startCountdown();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        waiter.start();
        Thread.sleep(1000); // Ensure the waiting thread starts first
        notifier.start();

        waiter.join();
        notifier.join();
    }
}
```


## Lock free programming using Atomic variables
At the core we have concept of CAS (Compare and Swap) which insures update occur only if it was not changed since last red  
Todos:
https://www.youtube.com/watch?v=vhHDlSV_0zg