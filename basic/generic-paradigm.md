泛型在使用尖括号“&lt;标识符1,标识符2,...&gt;”来表示，标识符代表的是某种类型。

泛型的作用其实是用它定义了一个模板，定义时并没有写死数据的类型，当真正使用的时候可以根据需要传入自己的数据类型。

**自定义泛型：**

```java
/*
   自定一个泛型只需要在类名之后使用<标识符>即可
   注意，此处的标识符是随意定义，就像变量名一样
  */
 public class MyGenericTest<T> {
     public static void main(String[] args) {
         // 定义的时候，传入的类型是什么，那么创建的对象使用的泛型类型就是什么类型
         MyGenericTest<String> mgt = new MyGenericTest<>();
         mgt.func("自定义泛型方法测试！");
     }
 
     /*
       如果想要使用泛型定义的类型，在方法参数中直接使用即可
      */
     public void func(T t){
         System.out.println(t);
     }
 }
```

**集合中泛型的应用：**

```java
import java.util.ArrayList;
 import java.util.Iterator;
 import java.util.List;
 
 
 public class GenericTest {
     public static void main(String[] args) {
         // 指定集合中的元素类型为Pet，不能存储其它类型的元素
         // 使用new的时候可以不用再传入类型了，可以自动推断，此时的表达式<>也称为钻石表达式
         // 如果不指定泛型，也是可以的，默认就是Object类型
         List<Pet> petList = new ArrayList<>();
         Cat c = new Cat();
         Dog d = new Dog();
 
         petList.add(c);
         petList.add(d);
 
         // 迭代器的声明也需要加上泛型的定义
         Iterator<Pet> it = petList.iterator();
         while (it.hasNext()) {
             // 原本next方法返回值的类型为Object，使用泛型之后返回的类型直接就是指定
             // 的类型，不需要进行类型转换了。
             Pet p = it.next();
             p.play();
             // 当然，如果要使用具体的子类对象的方法，还是需要转型之后才能调用
             if (p instanceof Cat){
                 Cat myCat = (Cat)p;
                 myCat.sleep();
             }
             if (p instanceof Dog){
                 Dog myDog = (Dog)p;
                 myDog.bark();
             }
         }
         /*
         输出结果：
         宠物在玩耍！
         猫咪在睡觉！
         宠物在玩耍！
         狗子在嚎叫！
         */
     }
 }
 
 
 class Pet {
     public void play() {
         System.out.println("宠物在玩耍！");
     }
 }
 
 
 class Cat extends Pet {
     public void sleep() {
         System.out.println("猫咪在睡觉！");
     }
 }
 
 
 class Dog extends Pet {
     public void bark() {
         System.out.println("狗子在嚎叫！");
     }
 }
```



