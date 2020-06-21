当线程之间某个线程想要获取的锁被对方线程占有了，与此同时，对方线程想要获取的锁也被自己获取了，此时两个线程就都会处于一直等待的状态，而不往下执行的情况，这种场景称之为死锁。

**注：**synchronized在开发中应该尽量避免嵌套使用，因为嵌套synchronized有可能会造成死锁问题，而死锁问题大多时候很难定位。

**死锁示例：**

```java
public class ThreadTest{
    public static void main(String[] args){
        Object o1 = new Object();
        Object o2 = new Object();
        MyThread1 t1 = new MyThread1(o1, o2);
        MyThread2 t2 = new MyThread2(o1, o2);
        t1.start();
        t2.start();
    }
}

class MyThread1 extends Thread{
    Object o1;
    Object o2;

    public MyThread1(Object o1, Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }

    public void run(){
        synchronized(o1){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 此时想要去获取o2的锁，但是已经被MyThread2的线程获取了，只能暂定并等待
            synchronized(o2){
                System.out.println("MyThread1 run...");
            }
        }
    }
}

class MyThread2 extends Thread{
    Object o1;
    Object o2;

    public MyThread2(Object o1, Object o2){
        this.o1 = o1;
        this.o2 = o2;
    }

    public void run(){
        synchronized(o2){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 此时想要去获取o1的锁，但是已经被MyThread1的线程获取了，只能暂定并等待
            synchronized(o1){
                System.out.println("MyThread2 run...");
            }
        }
    }
}
```



