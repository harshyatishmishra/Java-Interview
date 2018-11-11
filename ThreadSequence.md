```

  class Counter {
    public volatile int counter = 1;
  }
```
```
  class Thread_1 implements Runnable {
    static int count = 0;
    Counter thread1_counter;

    Thread_1(Counter counter) {
      thread1_counter = counter;
    }

    @Override
    public void run() {
      synchronized (thread1_counter) {
        for (int i = 0; i < 4; i++) {

          try {
            while (thread1_counter.counter != 1) {
              thread1_counter.wait();
            }

            System.out.println("A" + count);
            count++;
            Thread.sleep(1000);
            thread1_counter.counter = 2;
            thread1_counter.notifyAll();
          } catch (Exception e) {
            System.out.println("Exception in thread A");
            e.printStackTrace();
          }
        }
      }
    }
  }
```
```
  class Thread_2 implements Runnable {
    static int count = 0;
    Counter thread2_counter;

    Thread_2(Counter counter) {
      thread2_counter = counter;
    }

    @Override
    public void run() {
      synchronized (thread2_counter) {

        for (int i = 0; i < 4; i++) {
          try {
            while (thread2_counter.counter != 2) {
              thread2_counter.wait();
            }
            System.out.println("B" + count);
            count++;
            Thread.sleep(1000);
            thread2_counter.counter = 1;
            thread2_counter.notifyAll();
          } catch (Exception e) {
            System.out.println("Exception in thread B");
            e.printStackTrace();
          }
        }
      }
    }
  }
```
```
  public class ThreadSequence_2 {

    public static void main(String[] args) throws InterruptedException {
      Counter counter = new Counter();
      
      Thread_1 thread1 = new Thread_1(counter);
      Thread_2 thread2 = new Thread_2(counter);
      
      Thread thread_1 = new Thread(thread1);
      Thread thread_2 = new Thread(thread2);
      
      thread_1.start();
      thread_2.start();
    }

  }

```
