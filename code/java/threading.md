# Threads

Threads are computer processes.

- Run in the **background** and let the *main thread* *(actual program)* continue to run. These are called **daemon** threads.

- While they **run**, they **block** the *main thread* from continuing to run, until the **thread** is finished.

---

One **thread** class contains one **runnable**.

If you create the **runnable**, then you have to create the **thread** object.

If you create the class **thread**, then you can run it straight up.

>*Note: These threads are ***background*** threads.*

Please create separate **classes** for each **thread** in your code, so you can better **maintain** your program.

## Thread Class

```java
public class MyThread extends Thread 
{
  	// Overriding the run method
  	@Override
    public void run() 
    {
        // Your thread code
    }
}

public static void main(String[] args) {
    MyThread t1 = new MyThread();
    t1.start();
}
```

## Runnable (Interface)

```java
public class RunnableImpl implements Runnable 
{
    // Overriding the run Method
    @Override
    public void run()
    {
        // Your thread code
    }
}

public static void main(String[] args) {
    Thread t1 = new Thread(new RunnableImpl());
    t1.start();
}
```

## Thread Priority

You can give threads priority. The bigger the **number**, the bigger the **priority**.


```java
public class MyThread extends Thread 
{
  	// Overriding the run method
  	@Override
    public void run() 
    {
        // Your thread code
        // important code
    }
}

public class MyThread2 extends Thread 
{
  	// Overriding the run method
  	@Override
    public void run() 
    {
        // Your thread code
        // not that important code
    }
}

public static void main(String[] args) {
    Thread t1 = new MyThread();
    Thread t2 = new MyThread2();

    t1.setPriority(2);
    t2.setPriority(1);

    t1.start();
    t2.start();
}
```

## .start() vs .run() vs .sleep() vs .wait()

- `thread.start();` - Creates a new thread and the **run()** method is executed on the newly **created** thread. Defined in **java.lang.Thread** class.

- `thread.run();` - No new thread is created and the **run()** method is executed on the calling thread itself. Defined in **java.lang.Runnable** interface and must be overridden in the implementing class.

*See: https://www.geeksforgeeks.org/difference-between-thread-start-and-thread-run-in-java/*

- `thread.sleep();` - Pauses the current thread's execution. Throws **InterruptedException** if another thread interrupts during sleep.

*See: https://www.geeksforgeeks.org/thread-sleep-method-in-java-with-examples/*

- `thread.wait();` - Does **require** the thread to own the object’s monitor *(via ***synchronized*** block)*. Throws **IllegalMonitorStateException** if not in *synchronized* block. This is not a **Thread** function, but a `java.lang.Object` function.

