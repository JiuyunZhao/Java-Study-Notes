使用**java.util.Timer**就可以自己实现一个定时器。

可以从构造方法指定定时器的名称，以及该定时器是否作为守护线程运行。注意，这里说的定时器，包括了定时器本身线程和在定时器中运行的任务线程。

常用方法：

* **void schedule\(TimerTask task, Date firstTime, long period\)：**从指定的时间firstTime开始，每隔period（毫秒）时间执行一次任务task。注意，TimerTask是一个抽象类，使用时需要自己新写一个类，继承TimerTask并重写它的run方法。调用这个方法之后，定时器就开始工作了。



