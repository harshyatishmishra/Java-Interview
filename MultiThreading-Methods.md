### Yield
- yield’ means to let go, to give up, to surrender. A yielding thread tells the virtual machine that it’s willing to let other threads be scheduled in its place. This indicates that it’s not doing something too critical. Note that it’s only a hint, though, and not guaranteed to have any effect at all.
- the current thread is willing to relinquish its current use of processor but it'd like to be scheduled back soon as possible.
The “scheduler” is free to adhere or ignore this information and in fact, has varying behavior depending upon the operating system.

### yield() vs wait()
While yield() is invoked in the context of the current thread, wait() can only be invoked on an explicitly acquired lock inside a synchronized block or method
Unlike yield(), it is possible for wait() to specify a minimum time period to wait before any attempt to schedule the thread again
With wait() it is also possible to wake the thread anytime through an invocation of notify() or notifyAll() on the concerned lock object
### yield() vs sleep()
While yield() can only make a heuristic attempt to suspend the execution of the current thread with no guarantee of when will it be scheduled back, sleep() can force the scheduler to suspend the execution of the current thread for at least the mentioned time period as its parameter.
### yield() vs join()
The current thread can invoke join() on any other thread which makes the current thread wait for the other thread to die before proceeding
Optionally it can mention a time period as its parameter which indicates the maximum time for which the current thread should wait before resuming.

```
public class YieldExample
{
   public static void main(String[] args)
   {
      Thread producer = new Producer();
      Thread consumer = new Consumer();
       
      producer.setPriority(Thread.MIN_PRIORITY); //Min Priority
      consumer.setPriority(Thread.MAX_PRIORITY); //Max Priority
       
      producer.start();
      consumer.start();
   }
}
 
class Producer extends Thread
{
   public void run()
   {
      for (int i = 0; i < 5; i++)
      {
         System.out.println("I am Producer : Produced Item " + i);
         Thread.yield();
      }
   }
}
 
class Consumer extends Thread
{
   public void run()
   {
      for (int i = 0; i < 5; i++)
      {
         System.out.println("I am Consumer : Consumed Item " + i);
         Thread.yield();
      }
   }
}
```
```
I am Producer : Produced Item 0
 I am Consumer : Consumed Item 0
 I am Producer : Produced Item 1
 I am Consumer : Consumed Item 1
 I am Producer : Produced Item 2
 I am Consumer : Consumed Item 2
 I am Producer : Produced Item 3
 I am Consumer : Consumed Item 3
 I am Producer : Produced Item 4
 I am Consumer : Consumed Item 4
 ```
 
 
 ### Join
 
- The join() method of a Thread instance can be used to “join” the start of a thread’s execution to the end of another thread’s execution so that a thread will not start running until another thread has ended. If join() is called on a Thread instance, the currently running thread will block until the Thread instance has finished executing.

//Waits for this thread to die. 
 
``` public final void join() throws InterruptedException```
- Giving a timeout within join(), will make the join() effect to be nullified after the specific timeout. When the timeout is reached, the main thread and taskThread are equally probable candidates to execute. However, as with sleep, join is dependent on the OS for timing, so you should not assume that join will wait exactly as long as you specify.

```
package test.core.threads;
 
public class JoinExample
{
   public static void main(String[] args) throws InterruptedException
   {
      Thread t = new Thread(new Runnable()
         {
            public void run()
            {
               System.out.println("First task started");
               System.out.println("Sleeping for 2 seconds");
               try
               {
                  Thread.sleep(2000);
               } catch (InterruptedException e)
               {
                  e.printStackTrace();
               }
               System.out.println("First task completed");
            }
         });
      Thread t1 = new Thread(new Runnable()
         {
            public void run()
            {
               System.out.println("Second task completed");
            }
         });
      t.start(); // Line 15
      t.join(); // Line 16
      t1.start();
   }
}
 
Output:
 
First task started
Sleeping for 2 seconds
First task completed
Second task completed
```

### Java sleep() and wait() – Discussion
- sleep() is a method which is used to pause the process for few seconds or the time we want to. But in case of wait() method, thread goes in waiting state and it won’t come back automatically until we call the notify() or notifyAll().

- The major difference is that wait() releases the lock or monitor while sleep() doesn’t releases the lock or monitor while waiting. wait() is used for inter-thread communication while sleep() is used to introduce pause on execution, generally.

- Thread.sleep() sends the current thread into the “Not Runnable” state for some amount of time. The thread keeps the monitors it has acquired — i.e. if the thread is currently in a synchronized block or method no other thread can enter this block or method. If another thread calls t.interrupt(). it will wake up the sleeping thread.

- While sleep() is a static method which means that it always affects the current thread (the one that is executing the sleep method). A common mistake is to call t.sleep() where t is a different thread; even then, it is the current thread that will sleep, not the t thread.

- Method called on
wait() – Call on an object; current thread must synchronize on the lock object.
sleep() – Call on a Thread; always currently executing thread.
- Synchronized
wait() – when synchronized multiple threads access same Object one by one.
sleep() – when synchronized multiple threads wait for sleep over of sleeping thread.
- Lock duration
wait() – release the lock for other objects to have chance to execute.
sleep() – keep lock for at least t times if timeout specified or somebody interrupt.
- wake up condition
wait() – until call notify(), notifyAll() from object
sleep() – until at least time expire or call interrupt().

