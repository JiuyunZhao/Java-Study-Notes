### 1、对异常的理解

异常也是类，每一个异常类都可以创建异常对象。

异常继承结构：通过帮助文档中可以看到，java.lang.Object --&gt; java.lang.Throwable，而Throwable下有两个子类Error和Exception，它们都是可以抛出的，对于这两个子类分支，Error分支下的子类（包括Error自身）称之为错误，错误一旦发生，通常是直接就退出程序了，没有办法及时去处理。而Exception分支下的子类（包括Exception自身）称之为异常，异常是可以在代码层面提前去处理的，Exception的子类又可以分为两个分支，一个分支是RuntimeException及其子类，称为运行时异常，另一个分支就是除RuntimeException外的其它Exception的直接子类，也称为编译时异常。

编译时异常：之所以称之为编译时异常，是因为在编译阶段就可以发现并提醒程序员提前处理这种异常，对于这类异常怎么提前去处理，还是得要实际使用中多练才能有更深的体会。

运行时异常：这类异常在编译时不会报错，但是编译通过之后在运行时又会出错，所以叫运行时异常，比如对于表达式10/0，除数为0肯定是错的，但是编译器并不会识别并提醒，编译通过之后运行的时候肯定就会报错了。

异常处理：处理异常的方式有两种，一种是使用throws关键字和throw关键字，将异常抛出给上一级，让上一级去处理（上一级此时必须处理这个异常）；另一种是使用“try...catch”语句把异常捕获，但是注意，捕获到了这个异常不一定要去处理它，让它直接“过”也是允许的。



### 2、throws抛出异常

**throws使用示例：**

```java
public class ThrowsTest{
    public static void main(String[] args){
        // 这里在编译时会发生错误，也就是编译时异常，之所以有这个异常
        // 因为在func定义中有throws关键字，表示这个方法在执行过程中可能会发生
        // 对应的异常（ClassNotFoundException），所以它的上一级必须去处理
        // 这个异常，不处理的话，编译器就会不通过。
        func();
    }
    
    // 使用throws关键字抛出可能发生的异常
    public static void func() throws ClassNotFoundException{
        System.out.println("my func!!!");
        
        // 使用throw手动抛出一个异常
        throw new ClassNotFoundException("未找到类异常！");
    }
}
```

关于throws的使用，需要注意：

* throws抛出的异常，通常有两种处理方式，一种是继续使用throws关键字向上一级抛出同样的异常，即调用者自身不处理这个异常，让再上一级去处理。另一种是使用try...catch语句去捕获抛出的异常。
* Java内置类或者我们自己定义的类如果有使用throws关键字，就表示它是编译时异常，使用的时候必须要去处理它。
* throws抛异常时，既可以抛出具体的异常，也可以抛出它的某个父类异常，最顶级的异常类可以是Exception类，它包含了所有异常。
* 使用throws的时机就是如果这个异常希望调用者来处理，那么就是用throws抛出它，其他情况应该使用try捕获的方式。



### 3、try捕获异常

**try使用示例：**

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ExceptionTest {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try {
            // 将可能会发生异常的语句放在try的语句块中
            // try语句块中的代码一定会去执行，直到发生异常为止
            fis = new FileInputStream("Z:\\Study\\temp.md");
            System.out.println(10 / 0);
        } catch (ArithmeticException e) {  // 捕获可能会发生的异常
            // 捕获到异常后，对异常进行处理
            // catch语句块中的代码只有捕获到异常之后才会执行
            // ArithmeticException e这个语句相当于是声明一种引用类型的变量，类似于方法的形参，e保存的是异常对象的内存地址，并且可以在catch语句块中去使用这个对象引用e。
            System.out.println("发生算术异常！！！");
        } catch (FileNotFoundException e) {  // 可以使用多个catch语句块捕获不同的异常
            // 多个catch时会从上到下依次捕获，执行了一个catch之后，其他的catch就不会执行了
            // 并且多个catch语句时，应该遵循从上到下的“从小到大”捕获，即具体异常在前，它的父类型依次往后
            System.out.println("文件不存在！！！");
        } finally {
            // finally块中的语句无论是否发生异常都会处理，哪怕try块最后有个return语句
            // 执行到return语句时也会先执行finally中的语句，再执行return
            // 可以用来处理一些无论是否发生异常都要处理的操作，如关闭文件流等
            if (fis != null){
                try{
                    fis.close();
                } catch(IOException e) {
                    e.printStackTrace();
                }
                
            }
            System.out.println("关闭文件等其他操作。。。");
        }
    }
}
```

注意，catch捕获的异常可以是具体的异常，也可以是具体异常的父类型异常，此时可以理解为多态。

**异常对象中的常用方法：**

* **getMessage\(\)：**获取异常的简单描述信息。
* **printStackTrace\(\)：**打印异常追踪的堆栈信息。



### 4、自定义异常

自定义的异常类需要继承Exception或者RuntimeException，并且需要定义无参和有参两个构造方法。

```java
public class MyException extends Exception{
    public MyException{
        
    }
    public MyException(String s){
        super(s);
    }
}
```

自定义异常中的方法重写或者覆盖时需要注意一个语法问题，重写的方法抛出的异常不能比父类的方法更大或者说更宽泛，只能更小或者说更具体，比如父类异常方法抛出了IOException异常，那么异常子类中重写这个方法时就不能抛出Exception异常，但是可以抛出IOException异常或者FileNotFoundException异常。



