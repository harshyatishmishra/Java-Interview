
Synchronization is the process which keeps all concurrent threads in execution to be in sync. Synchronization avoids memory consistence errors caused due to inconsistent view of shared memory. When a method is declared as synchronized; the thread holds the monitor or lock object for that method’s object. If another thread is executing the synchronized method, your thread is blocked until that thread releases the monitor.

### Object level lock in Java

Object level lock is mechanism when we want to synchronize a non-static method or non-static code block such that only one thread will be able to execute the code block on given instance of the class. This should always be done to make instance level data thread safe.

###### Various ways for object level locking

```
public class DemoClass
{
    public synchronized void demoMethod(){}
}
 
or
 
public class DemoClass
{
    public void demoMethod(){
        synchronized (this)
        {
            //other thread safe code
        }
    }
}
 
or
 
public class DemoClass
{
    private final Object lock = new Object();
    public void demoMethod(){
        synchronized (lock)
        {
            //other thread safe code
        }
    }
}
```


### Class level lock in Java

Class level lock prevents multiple threads to enter in synchronized block in any of all available instances of the class on runtime. This means if in runtime there are 100 instances of DemoClass, then only one thread will be able to execute demoMethod() in any one of instance at a time, and all other instances will be locked for other threads.

Class level locking should always be done to make static data thread safe. As we know that static keyword associate data of methods to class level, so use locking at static fields or methods to make it on class level.

###### Various ways for class level locking

```
public class DemoClass
{
    //Method is static
    public synchronized static void demoMethod(){
 
    }
}
 
or
 
public class DemoClass
{
    public void demoMethod()
    {
        //Acquire lock on .class reference
        synchronized (DemoClass.class)
        {
            //other thread safe code
        }
    }
}
 
or
 
public class DemoClass
{
    private final static Object lock = new Object();
 
    public void demoMethod()
    {
        //Lock object is static
        synchronized (lock)
        {
            //other thread safe code
        }
    }
}
```

###### It’s possible that both static synchronized and non static synchronized method can run simultaneously or concurrently because they lock on different object.

###### According to the Java language specification you can not use synchronized keyword with constructor. It is illegal and result in compilation error.
###### Do not synchronize on non final field on synchronized block in Java. because reference of non final field may change any time and then different thread might synchronizing on different objects i.e. no synchronization at all. Best is to use String class, which is already immutable and declared final.


### Reentrant Lock

Java synchronized keyword is re-entrant in nature it means if a synchronized method calls another synchronized method which requires same lock then current thread which is holding lock can enter into that method without acquiring lock.
Allowing a thread to acquire the same lock more than once enables reentrant synchronization. This describes a situation where synchronized code, directly or indirectly, invokes a method that also contains synchronized code, and both sets of code use the same lock. Without reentrant synchronization, synchronized code would have to take many additional precautions to avoid having a thread cause itself to block.
