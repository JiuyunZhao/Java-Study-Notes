可以使用synchronized语法来实现线程之间的同步，以给某个代码块、方法或者类添加锁的方式，以达到数据安全的目的。

**代码块中的synchronized：**在代码块中使用synchronized，语法如下：

```java
/* 例如线程t1、t2、t3之间共享对象testShare，而需要同步的代码正好是testShare中的一个方法，
   那么synchronized就需要用在testShare中，
   小括号中的"线程之间共享的对象"就可以写this，而方法体中的代码就可以放在synchronized
   的大括号中来执行。这样，同一个类new出来的不同对象就可以实现各自的线程间同步，互不干扰。
   注意：线程之间共享需要共享的对象可能是不同的，而大括号中的代码和共享对象之间不一定是有关系的，
   这两个部分可以是没有关系的，所以这里不一定是this。这个共享对象只是给线程获取锁提供了一个对象，
   多个线程之间只有需要获取相同对象的锁的时候，才会发生线程的同步。
*/
synchronized(线程之间共享的对象){
    需要同步的代码
}
```

**synchronized原理：**synchronized语法实现线程之间同步的原理其实就是线程对对象锁的占有和释放，每一个Java对象都有一个锁（其实就是一个标记，我们称之为锁而已），当第一个线程遇到synchronized之后就会占有小括号中“共享对象”的锁，然后执行大括号中需要同步的代码块，如果在执行过程中，第二个线程也来到了这里，遇到了synchronized，也会去占有这个“共享对象”的锁，但是发现它已经被占有了，那么就只好排队等待，直到第一个线程执行完毕，释放这个“共享对象”的锁，然后第二个线程才能占有锁并继续执行后面的代码。以此类推，后面的线程也会来占有锁，如果锁已经被占有了，就停止执行并等待，直到“有锁可占”，如此，也就达到了这段代码的线程间同步。

**synchronized效率提升：

* 大括号中的内容越多，范围越大，执行效率越低，所以应该尽量保证大括号中的内容少一点，范围小一点。
* 对于局部变量，因为它始终都在栈中，而各自的线程都有自己的栈，所以局部变量是不存在线程安全问题的，因此，对于Java中的某些引用数据类型，在局部变量的使用中，应该使用非线程安全的数据类型，比如ArrayList、HashSet、StringBuilder等，它们虽然本身不是线程安全的，但是因为是局部变量，所以不存在线程安全问题，也就不用去考虑它们本身的线程安全问题了。

**方法定义中的synchronized：**synchronized可以在方法定义上使用，此时共享对象默认为this，同步的代码为整个方法体的代码。但是注意，如果是静态方法，那么执行这个方法时查找的锁就是类锁了，而不是对象锁了。这种用法虽然直接使用了this和整个方法体中的代码，但是也可以看情况使用，满足这个使用条件的就可以使用这个方式，代码也会更简洁。

```java
public synchronized void myFunc(){
    ....
}
```

**类定义中的synchronized：**如果synchronized关键字出现在类的定义修饰符中，那么表示这个类创建的所有对象都拥有同一个锁，也称之为类锁，类锁的定义主要是为了保证静态变量的线程安全。


