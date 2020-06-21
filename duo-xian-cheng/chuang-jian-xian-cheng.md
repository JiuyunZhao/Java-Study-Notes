### Thread方式

定义一个类，继承**java.lang.Thread**，并重写run方法即可。运行时，调用线程对象的start方法，然后JVM就会自动创建一个分支线程（分支栈）来运行run方法中的代码。这种方式也是最核心的，其他两种方式都是基于这个Thread来实现的。

Thread中的常用方法：

* **void start\(\)：**start\(\)方法的作用是启动一个分支线程，调用时会在JVM中开辟出一个新的栈空间，这个栈空间开辟出来后start\(\)方法就结束了，表示线程启动成功了。注意，start\(\)方法本身并不属于新的分支线程，而是属于调用者线程。start\(\)方法结束后，启动成功的线程会自动调用run方法，并且run方法处于分支栈的底部（压栈），其作用和意义就相当于是分支栈的main方法，即主线程的main方法和分支线程的run方法对于各自的线程来讲是意义一样的。
* **void run\(\)：**如果在当前线程中直接调用run方法，那它就是线程对象中的一个普通方法，并不会启动一个新的分支线程，所以想要启动一个新的分支线程，必须要通过调用start方法来运行run方法中的代码。
* **String getName\(\)：**获取线程的名称，默认为“Thread-\[n\]”，n表示数字。
* **void setName\(String name\)：**设置线程的名称。
* **static Thread currentThread\(\)：**获取当前线程的线程对象，当前线程指的是正在执行currentThread\(\)这个方法的线程。（注意这是个静态方法）
* **static void sleep\(long millis\)：**使当前线程暂停执行指定毫秒数。（注意这是个静态方法）
* **void interrupt\(\)：**中断sleep的睡眠。原理是调用sleep方法进行睡眠时，会产生一个InterruptedException的编译时异常，代码中通常会使用try块将sleep方法包裹起来，当调用interrupt方法时，就会主动抛出一个InterruptedException异常，此时的sleep睡眠就被中断了。
* **static void yield\(\)：**线程让位，让当前线程短暂的暂停一下，以便让其他线程得以有更多时间执行。（注意这是个静态方法）
* **void join\(\)：**线程合并，让当前线程阻塞，直到调用join方法的线程执行完毕，即让其他线程合入当前线程。
* **void setDaemon\(boolean on\)：**将on设置true传入，表示在线程调用start之前将其设置为守护线程，注意，这个方法需要在线程启动之前调用进行设置。Java中线程分为两类，用户线程和守护线程，守护线程也称为后台线程，而且守护线程通常是一个死循环程序，并且所有的用户线程结束之后，守护线程就会自动结束，不用程序员手动去结束。主线程main线程是属于用户线程，而垃圾回收机制的线程则属于守护线程。

**Thread简单示例：**

```java
public class ThreadTest{
    public static void main(String[] args){
        // main方法中的代码属于主线程，在主栈中运行
        MyThread myThread = new MyThread();
        // 调用线程对象的start方法会启动一个新的分支线程，并执行线程对象中run方法的代码
        // 此时主线程的main方法并不会等myThread的run方法运行完毕，而是会直接往下继续执行
        // 因为它们属于两个独立的线程，它们的运行是并行执行的
        myThread.start();
        System.out.println("主线程正在运行...");
    }
}


class MyThread extends Thread {
    public void run(){
        // run方法中的代码会运行在创建的分支线程中
        System.out.println("分支线程正在执行...");
    }
}
```



### Runnable方式

定义一个类，实现**java.lang.Runnable**接口，并重写接口的run方法，这个类也称之为可运行的类。然后再创建一个Thread对象，在创建Thread对象时，构造方法中将这个自定义的可运行类对象传入即可。

**注：**这种实现接口的方式其实更加常用，因为定义的可运行类在将来还可以继承别的类，但定义Thread子类的方式因为Java只支持单继承的原因就没有机会再继承别的类了，即无法通过继承的方式扩展功能了。

**Runnable简单示例：**

```java
public class ThreadTest{
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t = new Thread(myRunnable);
        // 启动分支线程，并在分支线程中运行myRunnable对象中的run方法
        t.start();
        System.out.println("主线程正在运行...");
    }
}

// 这只是一个实现了Runnable接口的普通类
// 只有将它传入Thread对象才能在单独的线程中运行
class MyRunnable implements Runnable{
    public void run(){
        System.out.println("分支线程正在执行...");
    }
}
```



### Callable方式

实现**java.util.concurrent.Callable**接口，并重写call\(\)方法，具体使用方法见示例。这种方式的特点是可以获取线程的返回值。但是，也有一个缺点，调用get方法获取返回值时会阻塞当前线程。

**Callable简单示例：**

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class CallableTest {
    public static void main(String[] args) throws Exception{
        // FutureTask使用了泛型，使用时可以传入自己需要的类型
        // 这里采用了匿名内部类的实现方式
        FutureTask task = new FutureTask(new Callable(){
            // 需要重写call方法，就相当于Thread中的run方法
            @Override
            public Object call() throws Exception {
                System.out.println("Callable线程正在运行...");
                return new Object();
            }
        });

        Thread t = new Thread(task);
        t.start();
        // 执行get方法是会阻塞当前线程（这里是主线程main），
        // 直到线程t执行完毕
        Object obj = task.get();
        System.out.println("线程执行的结果：" + obj);
    }
}
```





