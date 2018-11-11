```
  import java.util.LinkedList;

  class Inventory {
    LinkedList<Integer> queue = new LinkedList<Integer>();
    int capacity;

    public Inventory(int i) {
      this.capacity = i;
    }

    public void produce() throws InterruptedException {
      int value = 0;
      while (true) {
        synchronized (this) {
          if (queue.size() == capacity) {
            wait();
          }
          queue.add(value);
          System.out.println("Produced : " + value + " Size: "+ queue.size());
          value++;
          notify();
          Thread.sleep(1000);
        }
      }
    }

    public void consumer() throws InterruptedException {
      while (true) {
        synchronized (this) {
          if (queue.size() == 0) {
            wait();
          }
          System.out.println("Consumer : " + queue.removeFirst()+ " Size: "+ queue.size());
          notify();
          Thread.sleep(1000);
        }
      }
    }
  }
```
```
  class Producer implements Runnable {

    private Inventory inventory;

    public Producer(Inventory inventory) {
      this.inventory = inventory;
    }

    @Override
    public void run() {

      try {
        inventory.produce();
      } catch (Exception e) {
        System.out.println("Exception in Producer");
        e.printStackTrace();
      }
    }
  }
```
```
  class Consumer implements Runnable {
    private Inventory inventory;

    public Consumer(Inventory inventory) {
      this.inventory = inventory;
    }

    @Override
    public void run() {
      try {
        inventory.consumer();
      } catch (Exception e) {
        System.out.println("Exception in Consumer");
        e.printStackTrace();
      }
    }
  }
```
```
  public class ProducerConsumerUsingLinkedList {

    public static void main(String[] args) throws InterruptedException {
      Inventory inventory = new Inventory(5);
      
      Producer producer = new Producer(inventory);
      Consumer consumer = new Consumer(inventory);
      
      Thread producerThread = new Thread(producer);
      Thread consumerThread = new Thread(consumer);

      producerThread.start();
      consumerThread.start();
    }

  }

```
