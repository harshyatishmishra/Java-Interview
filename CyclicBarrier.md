- A CyclicBarrier is a synchronizer that allows a set of threads to wait for each other to reach a common execution point, also called a barrier.

- CyclicBarriers are used in programs in which we have a fixed number of threads that must wait for each other to reach a common point before continuing execution.

- The constructor for a CyclicBarrier is simple. It takes a single integer that denotes the number of threads that need to call the await() method on the barrier instance to signify reaching the common execution point:

1. CyclicBarrier can perform a completion task once all thread reaches to the barrier, This can be provided while creating CyclicBarrier.

2. If CyclicBarrier is initialized with 3 parties means 3 thread needs to call await method to break the barrier.
3. The thread will block on await() until all parties reach to the barrier, another thread interrupt or await timed out.
4. If another thread interrupts the thread which is waiting on barrier it will throw BrokernBarrierException as shown below:
```
java.util.concurrent.BrokenBarrierException
        at java.util.concurrent.CyclicBarrier.dowait(CyclicBarrier.java:172)
        at java.util.concurrent.CyclicBarrier.await(CyclicBarrier.java:327)
```
5. CyclicBarrier.reset() put Barrier on its initial state, other thread which is waiting or not yet reached barrier will terminate with java.util.concurrent.BrokenBarrierException.

```
import java.util.Date;
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

/**
 * CyclicBarriers are synchronization constructs that were introduced with Java
 * 5 as a part of the java.util.concurrent package
 * 
 * 
 * The barrier is called cyclic because it can be re-used after the waiting threads are released.
 * 
 * 
 * @author Harsh
 *
 *         CyclicBarrier, Phaser, CountDownLatch, Exchanger Semaphore,
 *         SynchronousQueue
 *
 */
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        
        //3 threads are part of the barrier, ServiceOne, ServiceTwo and this main thread calling them.
        final CyclicBarrier barrier = new CyclicBarrier(3);
         
        Thread serviceOneThread = new Thread(new ServiceOne(barrier));
        Thread serviceTwoThread = new Thread(new ServiceTwo(barrier));
         
        System.out.println("Starting both the services at"+new Date());
         
        serviceOneThread.start();
        serviceTwoThread.start();
         
        try {
            barrier.await();
        } catch (InterruptedException e) {
            System.out.println("Main Thread interrupted!");
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            System.out.println("Main Thread interrupted!");
            e.printStackTrace();
        }
        System.out.println("Ending both the services at"+new Date());
    }
}


class ServiceOne implements Runnable {
	 
    private final CyclicBarrier cyclicBarrier;
 
    public ServiceOne(CyclicBarrier cyclicBarrier) {
        this.cyclicBarrier = cyclicBarrier;
    }
 
    @Override
    public void run() {
        System.out.println("Starting service One...");
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }
        System.out
                .println("Service One has finished its work... waiting for others...");
        try {
            cyclicBarrier.await();
        } catch (InterruptedException e) {
            System.out.println("Service one interrupted!");
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            System.out.println("Service one interrupted!");
            e.printStackTrace();
        }
        System.out.println("The wait is over, lets complete Service One!");
 
    }
 
}


class ServiceTwo implements Runnable {
    
    private final CyclicBarrier cyclicBarrier;
 
    public ServiceTwo(CyclicBarrier cyclicBarrier) {
        this.cyclicBarrier = cyclicBarrier;
    }
 
    @Override
    public void run() {
        System.out.println("Starting service Two....");
         
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }
        System.out.println("Service Two has finished its work.. waiting for others...");
        try {
            cyclicBarrier.await();
        } catch (InterruptedException e) {
            System.out.println("Service one interrupted!");
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            System.out.println("Service one interrupted!");
            e.printStackTrace();
        }
        System.out.println("The wait is over, lets complete Service two!");
 
    }
 
}
```


```
Starting both the services atThu Dec 26 13:03:24 IST 2019
Starting service One...
Starting service Two....
Service One has finished its work... waiting for others...
Service Two has finished its work.. waiting for others...
The wait is over, lets complete Service two!
The wait is over, lets complete Service One!
Ending both the services atThu Dec 26 13:03:29 IST 2019
```
