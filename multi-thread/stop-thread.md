### 使用布尔标记终止线程

```java
// 终止线程的一种方式：定义一个布尔标记
public class ThreadTest{
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t = new Thread(myRunnable);
        t.start();

        // 主线程暂停5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 主线程暂停5秒之后，手动去终止t线程
        myRunnable.run = false;
    }
}


class MyRunnable implements Runnable{
    // 定义一个布尔标记
    boolean run = true;
    public void run(){
        // 让当前线程sleep 10秒，模拟程序执行10秒
        for (int i = 0; i < 10; i++) {
            if (run) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            } else {
                // 终止线程
                return;
            }
        }
    }
}
```



