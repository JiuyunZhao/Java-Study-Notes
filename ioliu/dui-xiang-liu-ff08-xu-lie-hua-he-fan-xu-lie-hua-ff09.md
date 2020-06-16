**序列化和反序列化：**对象流的使用涉及到两个概念，即序列化和反序列化，序列化（Serialize）是指将java对象存储到文件中，将java对象的状态以文件的形式保存下来。而反序列化（Deserialize）则是序列化的反过程，即将文件中的数据恢复到内存当中，恢复为java对象的过程。序列化的过程需要使用到ObjectOutputStream，而反序列化的过程需要使用到ObjectInputStream。

**ObjectOutputStream：**构造方法中传入一个FileOutputStream对象，然后调用writeObject方法就可以将一个需要序列化的java对象保存到指定的文件中了。如果需要序列化多个对象，可以将这些对象放到ArrayList集合中，最后将这个集合传入writeObject方法即可，但是需要注意一点，就是ArrayList集合本身也是实现了Serializable接口的，也就是说，使用的集合以及集合中的元素都需要实现Serializable接口才能进行序列化的操作。关于序列化多个对象，建议直接使用集合的方式就好了，如果采用多次调用writeObject的方式每次写入一个对象，这种方式是不允许的。

**ObjectInputStream：**构造方法中传入一个FileInputStream对象，然后调用readObject方法就可以从指定文件的反序列化结果中读取一个java对象（普通java对象或集合对象）到内存中了。

**Serializable接口：**参与序列化和反序列化的对象需要实现Serializable接口，但是注意，这个接口并没有任何方法，它只是起到了一个标识的作用（这种接口也称为标志接口），通过这个接口告诉JVM此类的对象可能会进行序列化的操作。有了这个标志接口，编译时JVM就会为这个java对象自动生成一个序列化版本号，这个序列化版本号的作用就是反序列化时用来识别当下源代码中的class定义和之前执行序列化操作时的class定义是否一致，如果不一致则会报错，不能进行反序列化的操作，即反序列化的对象的类在当前源代码中的定义已经被修改，无法反序列化（反序列化回来时也需要一个对应class类型的变量去接收，两者不一样的话，自然就无法通过了），所以由此看出，这个序列化版本号本质上就是用来区分类的，判断两个类的定义是否相同。通常建议这个序列化版本有我们程序员自己手动定义，而不是让JVM自动生成。

**transient关键字：**序列化时，如果希望类中的某个字段不要将它序列化到文件中，只需要在属性定义的时候加上这个transient关键字即可。

**手动生成序列化版本号：**通常来讲，一个类定义之后，不可能保证它永远都不会被修改，但是当它被修改之后，它的序列化版本号就变了，这就会导致之前序列化到文件中的数据不能反序列化回来了，这样的结果通常是不行的，所以建议需要序列化的类都自己手动编写一个序列化版本号，而不是采用JVM自动生成的方式。手动编写的方式就是在类定义中添加一个序列化版本号的属性即可“private static final long serialVersionUID = 233L;”，这里的属性值通常来讲需要是保证它是项目中唯一的编号即可，所以可以根据项目情况自定义就可以了，如果不放心，自己生成一个全球唯一的UID也可以（IDEA也是可以自动生成这个属性的，在设置界面直接搜索serialVersionUID，可以看到Inspections下有个对应的检查项“Serializable class without 'serialVersionUID'”，勾上就可以了）。

**使用示例：  
**

普通java对象

```java
// 实现Serializable，但这个接口没有任何方法需要去重写
public class User implements Serializable {
    private int id;
    private String name;
    ...
}
```

序列化单个java对象

```java
User u = new User(123, "zhangsan");
// 将user对象序列化到Z:\\userinfo文件中
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("Z:\\userinfo"));
oos.writeObject(s);
oos.flush();
oos.close();
```

序列化多个java对象

```java
// 这里使用ArrayList存放多个User对象
List<User> userList = new ArrayList<>();
userList.add(new User(111, "zhangyi"));
userList.add(new User(222, "zhanger"));
userList.add(new User(333, "zhangsan"));

ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("Z:\\userinfo"));
oos.writeObject(userList);
oos.flush();
oos.close();
```

反序列化java对象

```java
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("Z:\\userinfo"));
// 反序列化出来的对象需要进行强制类型转换一下
List<User> userList = (List<User>)ois.readObject();
ois.close();
```



