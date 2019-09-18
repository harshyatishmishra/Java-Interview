```
import java.util.concurrent.LinkedBlockingQueue;

class CustomThreadPool {
    private int nThread;
    private LinkedBlockingQueue<Runnable> queue;
    private PoolWorker[] threads;

    public CustomThreadPool(int nThread) {
        this.nThread = nThread;
        queue = new LinkedBlockingQueue<>();
        threads = new PoolWorker[nThread];
        for (int i = 0; i < nThread; i++) {
            threads[i] = new PoolWorker();
            threads[i].start();
        }
    }

    public void execute(Runnable task) {
        synchronized (queue) {
            queue.add(task);
            queue.notify();
        }
    }

    private class PoolWorker extends Thread {
        Runnable task;

        @Override
        public void run() {
            try {
                while (true) {
                    synchronized (queue) {
                        while (queue.isEmpty()) {
                            queue.wait();
                        }
                        task = queue.poll();
                    }
                    task.run();
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadPool extends Thread {

    @Override
    public void run() {
        System.out.println("Get Data............" + Thread.currentThread().getName());
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        CustomThreadPool threadPool = new CustomThreadPool(10);
        threadPool.execute(new ThreadPool());
        threadPool.execute(new ThreadPool());
        threadPool.execute(new ThreadPool());
        threadPool.execute(new ThreadPool());
        threadPool.execute(new ThreadPool());
    }
}

```
