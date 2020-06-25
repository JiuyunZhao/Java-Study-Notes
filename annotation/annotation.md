

### 1. 定义注解（Annotation）

注解，或者称之为注释，也是一种引用数据类型，编译之后也会生成class文件，具体用法见示例：

```java
[修饰符列表] @interface 注解类型名{
    // 属性定义
}
```

**注解定义示例：**

```java
// 无属性的注解定义
public @interface MyAnnotation{
    // 这里面什么都不写，表示没有属性
}


// 有属性的注解定义
public @interface MyAnnotation2{
    // 定义一个没有默认值的属性，在使用这个注解的时候就必须给这个属性传值
    // 注意，注解的属性定义是有小括号的，但它不是方法，就只是属性
    String name();
    
    // 使用default给属性指定默认值，有默认值的属性在使用时就可以不用给这个属性传值了
    int id() default 2333;
}


// 属性只有一个，且为value时，使用时可以不用指定属性名称
public @interface MyAnnotation3{
    String value();
}
```

**注解使用示例：**

```java
// 使用：直接在类、方法、属性、形参、注解等上面使用形如”@注解类型名“的格式即可。
// 注解的使用其实就像修饰符一样在定义的前面加上就可以，但是通常的使用习惯是在定义上一行进行添加
public class AnnotationTest {
    public static void main(String[] args) {

    }

    // 相当于：@MyAnnotation private int id;
    @MyAnnotation
    private int id;

    // 注解只有一个属性，且属性名为value时，可以不用指定属性名
    @MyAnnotation3("hello")
    public AnnotationTest() {
    }

    // 定义了没有默认值的属性的注解，就必须给这个属性传值，有默认值的属性可以传，也可以不传
    @MyAnnotation2(name = "zhangsan")
    public static void func() {
        @MyAnnotation2(name = "lisi", id = 666)
        int i = 10;
    }

    // 相当于：@MyAnnotation public void func2(@MyAnnotation String name)
    @MyAnnotation
    public void func2(@MyAnnotation String name) {
        System.out.println(name);
    }
}
```

**注解属性类型：**定义注解的属性时，属性的类型可以是byte、short、int、long、float、double、boolean、char、String、Class、枚举类型，以及这几种类型的数组形式，不能是其他的类型。有一个小技巧，属性如果是数组，并且使用时传入的数组元素只有一个的话，定义数组的大括号是可以不写的。



### 2. Java内置注解

内置注解在java.lang包下，常用的有：

* **@Override：**这个注解只能注解方法，并且只是给编译器在编译阶段做参考用的，和运行阶段的代码没有关系，编译器在编译时会检查这个方法是否是重写的父类方法，如果不是则会报错。
* **@Deprecated：**表示被标注的类、方法等元素已经过时了，不建议使用。在IDEA中，被标注的方法等会出现一条横线，提示你这个方法已过时。



### 3. 元注解

用来标注“注解类型”的注解，即注解的注解，称之为元注解，在java.lang.annotation包下。常用的元注解有：

* **@Target\(ANNOTATION\_TYPE\)：**用来指定被标注的注解可以出现在哪些位置上，参数为枚举类型ElementType的数组，具体有哪些枚举值可以参考帮助文档，如“@Target\(ElementType.METHOD\)”表示被标注的注解只能出现在方法上。
* **@Retention\(RUNTIME\)：**用来指定被标注的注解最终保存在哪里，参数是一个枚举类型RetentionPolicy，有三个枚举值，“@Retention\(RetentionPolicy.SOURCE\)”表示被标注的注解保存在java源文件中，并不会出现在编译之后的class文件中，“@Retention\(RetentionPolicy.CLASS\)”表示被标注的注解保存在class文件中，“@Retention\(RetentionPolicy.RUNTIME\)”表示被标注的注解保存在class文件中，并且可以被反射机制读取出来。



### 4. 反射注解

以类的注解为例，首先获取到类的字节码对象后，使用Class对象的方法进行反射，常用的方法有：

* **boolean isAnnotationPresent\(MyAnnotation.class\)：**判断一个类是否有指定的注解，这里需要传入一个指定注解的类字节码对象。
* **getAnnotation\(MyAnnotation.class\)：**获取类的指定注解对象，这里需要传入一个指定注解的类字节码对象。

**属性获取：**通过反射拿到注解对象之后，就可以通过调用方法（其实是属性）的形式获取属性值，因为注解定义属性时，本身就自带小括号，所以看起来就是在调用方法了，如“String name = annotationObj.name\(\);”

**注：**方法、属性等的注解获取也是和上面的方法一样，而且调用的方法等大多也都是一样的。



### 5. 注解的作用

注解通常是通过反射机制去检查被注解的类、方法等是否满足要求，比如@Override就是检查被注解的方法是否是重写父类的方法，如果不是就会编译报错。



