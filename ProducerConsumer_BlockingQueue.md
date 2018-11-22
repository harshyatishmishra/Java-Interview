```

  import java.util.concurrent.BlockingQueue;
  import java.util.concurrent.LinkedBlockingQueue;

  class Producer implements Runnable {
    private final BlockingQueue blockingQueue;

    public Producer(BlockingQueue blockingQueue) {
      super();
      this.blockingQueue = blockingQueue;
    }

    @Override
    public void run() {

      for (int i = 0; i < 10; i++) {
        try {
          System.out.println("Produced: " + i);
          blockingQueue.put(i);
        } catch (Exception e) {
          System.err.println("Exception Occured in Producer");
        }
      }
    }
  }

  class Consumer implements Runnable {
    private final BlockingQueue blockingQueue;

    public Consumer(BlockingQueue blockingQueue) {
      super();
      this.blockingQueue = blockingQueue;
    }

    @Override
    public void run() {

      try {
        System.out.println("Produced: " + blockingQueue.take());
      } catch (Exception e) {
        System.err.println("Exception Occured in Consumer");
      }
    }
  }

  public class ProducerConsumerPatternn {

    public static void main(String[] args) {
      BlockingQueue sharedQueue = new LinkedBlockingQueue();

      Thread prodThread = new Thread(new Producer(sharedQueue));
      Thread consThread = new Thread(new Consumer(sharedQueue));

      // Starting producer and Consumer thread
      prodThread.start();
      consThread.start();

    }

  }

```
