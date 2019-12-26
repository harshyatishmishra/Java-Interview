- CountDown latch is a kind of synzhronizer which allows ont thread to wait for one or more thread before start processing.
- Introduced in Java 5
- CountDownLatch works on latch principle, thread will wait until gate is open. One thread waits for n number of threads specified while creating CountDownLatch.

e.g. `final CountDownLatch latch = new CountDownLatch(3);`

Here we set the counter to 3.

Any thread, usually main thread of application, which calls CountDownLatch.await() will wait until count reaches zero or it's interrupted by another Thread. All other threads are required to do count down by calling CountDownLatch.countDown() once they are completed or ready to the job. as soon as count reaches zero, the Thread awaiting starts running.

- Disadvantage
 Not usable when reached to Zero 
 
 -Any thread, usually the main thread of the application, which calls CountDownLatch.await() will wait until count reaches zero or it's interrupted by another thread. All other threads are required to count down by calling CountDownLatch.countDown() once they are completed or ready.

- As soon as count reaches zero, the waiting thread continues. 
- Use CountDownLatch when one thread (like the main thread) requires to wait for one or more threads to complete, before it can continue processing.

- A classical example of using CountDownLatch in Java is a server side core Java application which uses services architecture, where multiple services are provided by multiple threads and the application cannot start processing until all services have started successfully.

```

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * CountDownLatch is used to make sure that a task waits for other threads
 * before it starts. To understand its application, let us consider a server
 * where the main task can only start when all the required services have
 * started.
 * 
 * Working of CountDownLatch: When we create an object of CountDownLatch, we
 * specify the number of threads it should wait for, all such thread are
 * required to do count down by calling CountDownLatch.countDown() once they are
 * completed or ready to the job. As soon as count reaches zero, the waiting
 * task starts running
 * 
 * @author Harsh Mishra
 *
 */
public class CountDownLatchDemo {
	public static void main(String args[]) throws InterruptedException {
		// Let us create task that is going to
		// wait for four threads before it starts
		CountDownLatch latch = new CountDownLatch(4);

		// Create Executor Service for thread
		ExecutorService executorService = Executors.newFixedThreadPool(4);
		// Let us create four worker
		// threads and start them.
		int delay = 1000;
		for (int i = 0; i < 4; i++) {

			executorService.submit(new Worker(delay, latch, "WORKER-" + i + 1));
			delay += 1000;
		}

		// The main task waits for four threads
		latch.await();

		// Main thread has started
		System.out.println(Thread.currentThread().getName() + " has finished");
	}
}

// A class to represent threads for which 
// the main thread waits. 
class Worker extends Thread {
	private int delay;
	private CountDownLatch latch;

	public Worker(int delay, CountDownLatch latch, String name) {
		super(name);
		this.delay = delay;
		this.latch = latch;
	}

	@Override
	public void run() {
		try {
			System.out.println(Thread.currentThread().getName() + " started");
			Thread.sleep(delay);
			latch.countDown();
			System.out.println(Thread.currentThread().getName() + " finished");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```
