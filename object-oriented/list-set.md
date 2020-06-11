集合是一种容器对象，是用来存储对象的，或者说存储的都是对象的引用，并不是直接存储的对象，而是存储的对象的内存地址。需要注意的是，集合中不能存储基本数据类型，即使是代码中往集合中添加了基本数据类型，那也是进行了自动装箱之后才放入集合的。

需要注意的是，Java中每一种不同的集合，底层会对应不同的数据结构，所以应该根据实际情况选择使用合适的集合类型。

所有的集合都在“java.util”中，导入的时候去util包里找即可。

**注：**集合的定义中会经常看到&lt;E&gt;等尖括号表示的语法，这是泛型的定义，表示可以根据需要传入自己需要的类型，如果不传入，默认就是Object类型。即如果传入指定的类型，则集合中只能存储指定的类型，否则集合中可以存储Object类的所有子类型。



### 1、Collection集合继承结构

Collection类型的集合特点是以单个对象的个体方式存储元素。

**java.lang.Object --&gt; java.util.AbstractCollection&lt;E&gt;**这个类实现了以下两个接口：

* **Iterable&lt;E&gt;：**这个接口用于获取遍历集合所需要的迭代器。
* **Collection&lt;E&gt;：**这是所有集合的超级父接口，里面定义许多集合通用的方法。

**Collection&lt;E&gt;**接口中的常用方法：

* **boolean add\(E e\)：**向集合末尾添加一个元素。
* **int size\(\)：**返回集合中元素的个数。注意，不是返回集合的容量，而是实际存储的元素个数。
* **void clear\(\)：**清空集合中的所有元素。
* **boolean contains\(Object o\)：**判断当前集合中是否包含某个元素。这个方法其实是调用了元素o对象的equals方法来比较集合中的每个元素，如果有返回true的结果，则contains方法则返回true，否则返回false。
* **boolean remove\(Object o\)：**删除集合中的某个元素。这个方法也会调用o对象的equals方法来进行判断。
* **boolean isEmpty\(\)：**判断集合是否为空。
* **Iterator&lt;E&gt; iterator\(\)：**返回一个迭代器，需要注意的是，返回的迭代器一开始并没有直接就指向了第一个元素。另一个需要注意的点，就是集合的结构或者内容发生改变时，迭代器也需要重新获取，所以如果在迭代的过程中需要删除元素，那么就不能使用集合对象本身的remove方法，需要使用迭代器自身的remove方法。（返回的迭代器从原理上可以理解为当前集合状态的一个快照，迭代的时候只是会参照这个快照进行迭代，所以如果集合本身发生了变化，那么就和快照不一致了，此时再按照快照的状态去迭代集合元素就会发生异常。）
* **注：**因为contains和remove方法都会用到equals方法，所以需要保证往集合中添加的元素都已经重写了equals方法，特别是自己定义的对象。

**Iterator&lt;E&gt; iterator\(\)**中常用的方法：

* **boolean hasNext\(\)：**如果仍有元素可以迭代，则返回true，否则返回false。
* **E next\(\)：**返回迭代的下一个元素。这里需要注意，如果没有指定泛型，返回值类型是Object，虽然存储数据的时候可能是别的具体类型，但是获取数据的时候必须是Object。
* **default void remove\(\)：**删除迭代器指向的当前元素，即next方法返回的那个元素。（原理可以理解是，先删除快照中的当前元素，然后再同步删除集合中对应的元素。）
* **注：**通常先使用hasNext方法判断是否可以继续迭代，如果可以就使用next方法返回下一个元素， 注意，通过Iterator\(\)方法刚返回的迭代器并没有指向第一个元素，只有使用next方法之后才会指向第一个元素并往后迭代。）

**Collection&lt;E&gt;**接口下常用的子接口：

* **List&lt;E&gt;：**这个接口下的集合的特点是集合元素有序可重复，并且可以通过下标访问集合元素。实现这个接口的常用的类有ArrayList、LinkedList和Vector。
* **Set&lt;E&gt;：**这个接口下的集合的特点是集合元素无序不可重复，并且不可以通过下标访问集合元素。实现这个接口的常用的类有HashSet和TreeSet。

**List&lt;E&gt;**接口中的常用方法：

* **boolean add\(E e\)：**在集合末尾添加一个元素。
* **void add\(int index, E element\)：**在列表的指定位置添加一个元素。这个方法使用的较少，因为效率较低。
* **E get\(int index\)：**获取指定位置的元素。
* **int indexOf\(Object o\)：**获取某个元素第一次出现的索引。
* **int lastIndexOf\(Object o\)：**获取某个元素最后一次出现的索引。
* **E remove\(int index\)：**删除指定位置的元素。
* **E set\(int index, E element\)：**修改指定位置的元素。

### 2、Map集合继承结构

Map类型的集合特点是以key和value键值对的方式来存储集合元素的，即一个键值对算作一个集合元素。

**java.util.Map&lt;K,​V&gt;**接口中常用的方法：

* **V put\(K key, V value\)：**向Map集合中添加一个键值对。
* **V get\(Object key\)：**通过key获取value。
* **void clear\(\)：**清空Map集合。
* **boolean containsKey\(Object key\)：**判断Map集合中是否包含某个key。注意底层方法都是调用的equals方法比对的。
* **boolean containsValue\(Object value\)：**判断Map集合中是否包含某个value。注意底层方法都是调用的equals方法比对的。
* **boolean isEmpty\(\)：**判断Map集合中元素个数是否为0。
* **Set&lt;K&gt; keySet\(\)：**获取Map集合中所有的key。
* **Collection&lt;V&gt; values\(\)：**获取集合中所有的value。
* **V remove\(Object key\)：**通过key删除键值对。
* **int size\(\)：**获取Map集合中键值对的个数。
* **Set&lt;Map.Entry&lt;K, V&gt;&gt; entrySet\(\)：**将Map集合转换为Set集合，Set集合中元素的类型是“Map.Entry&lt;K, V&gt;”，相当于是将Map集合中每个元素的key和value联合起来当成了Set集合中的一个元素了。

**java.util.Map&lt;K,​V&gt;**接口的常用实现类：

* **HashMap&lt;K,​V&gt;：**这是Map下常用的集合，特点是集合的key无序不可重复，但是value可以重复。
* **Hashtable&lt;K,​V&gt; --&gt; Properties：**Hashtable本身和HashMap的作用类似，但是Hashtable是线程安全的，由于这个线程安全的实现方式导致效率较低，所以用的较少了，用的较多的是它的子类Properties。
* **SortedMap&lt;K,​V&gt; --&gt; TreeMap&lt;K,​V&gt;：**TreeMap&lt;K,​V&gt;也是一个Map集合下常用的集合，特点是除了集合的key无序不可重复外，由于实现了SortedMap&lt;K,​V&gt;接口，集合的key在存放进去后会自动排序。

 

### 3、ArrayList&lt;E&gt;

**特点：**集合元素有序可重复，可通过下标访问集合元素，并且是非线程安全的。

**方法：**Collection&lt;E&gt;和List&lt;E&gt;接口中的方法ArrayList&lt;E&gt;都可以使用。

**线程安全：**如果想要将ArrayList&lt;E&gt;变为线程安全的，可以通过“java.util.Collections”的synchronizedList\(yourList\)方法将你的ArrayList对象传入，之后的ArrayList就会变成线程安全的了。

**底层原理：**ArrayList底层采用的是Object类型数组的数据结构，创建对象时可以传入数组的大小，如果没有传入数组大小，则会在初始化时创建一个空列表，然后在第一次往里面添加数据时才会创建一个大小为10的数组。在往数组中添加数据时，如果容量满了，那么数组会自动进行扩容，并且扩容之后的数组容量为原容量的1.5倍。

**优缺点：**就和数组的优缺点一样，查询效率很快，往末尾添加数据也很快，但是随机增删元素的效率较低，所以使用的使用应该尽量避免随机增删以及扩容的操作。

**使用示例：**

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

### 4、LinkedList&lt;E&gt;

**特点：**集合元素有序可重复，可通过下标访问集合元素。

**方法：**Collection&lt;E&gt;和List&lt;E&gt;接口中的方法ArrayList&lt;E&gt;都可以使用。

**底层原理：**底层采用的是双向链表的数据结构，存储的时候会将元素封装到一个Node对象中，这个Node对象除了有元素本身外，还有两个变量next和prev，用于下一个节点和上一个节点的地址，这样就形成了一个“双向”的链表了。虽然是链表，但是还是可以使用下标，但是不推荐使用下边进行查询，因为每次查询都得从头结点开始，效率较低。

**优缺点：**查询效率较低，因为每次都要从头结点开始查询，但是随机增删元素的效率较高，因为只会涉及到上一个节点和下一个节点中变量next和prev的修改，不会像数组那样会涉及到该元素之后的值的整体移动。所以在选择使用上，如果随机增删的业务较多，应该使用LinkedList，如果需要往集合中频繁的添加或查询数据，就使用ArrayList。

**容量：**LinkedList因为是链表的数据结构，所以在内存空间上是没有连续性的，并且也没有容量的说法，刚开始都是空的，没有任何元素。



### 5、Vector&lt;E&gt;

Vector和ArrayList一样底层都是数组的数据结构，在使用上也是一样的，区别在于Vector是线程安全的，但是也是因为这点，导致效率较低，而ArrayList可以使用其他更好的方法来保证线程安全，所以Vector已经使用的较少了。



### 6、HashSet&lt;E&gt;

**特点：**集合元素无序不可重复，不可通过下标访问集合元素。

**方法：**Collection&lt;E&gt;接口中的方法ArrayList&lt;E&gt;都可以使用。

**底层原理：**HashSet底层其实是采用HashMap类型来存储元素的，不过只是用到了HashMap的key部分，value部分并没有用到，HashMap的key本身就是无序不可重复的，所以可以看成是HashMap所有的Key组成了一个HashSet。所以想要更好的理解HashSet，参见HashMap部分的笔记。



### 7、TreeSet&lt;E&gt;

**特点：**集合元素无序不可重复，不可通过下标访问集合元素，但是存放到集合中的数据都是按升序自动排好序的。

**方法：**Collection&lt;E&gt;接口中的方法ArrayList&lt;E&gt;都可以使用。

**底层原理：**由于TreeSet实现了SortedSet，所以存放进去的元素都是拍好序的，“无序不可重复”中的无序指的是存入进去的顺序和取出来的顺序不一致，这和存入进去之后自动排序是不一样的，不要搞混了。和HashSet类似，TreeSet底层其实使用Map下的TreeMap来存储数据的，它也是只用到了TreeMap的key部分，没有用到value部分，所以理解TreeSet，可以参见TreeMap部分的笔记。

**自动排序：**为了改变自动升序为自动降序排列，或者存储自定义的类型的时候，需要重写或自定义TreeSet的排序机制，特别是自定义的类型，如果没有同时定义好排序机制的话是会报错的，自定义排序机制为：给这个自定义的类实现“java.lang.Comparable”接口或者自定义一个比较器“java.util.Comparator”。示例如下：

```java
import java.util.Comparator;
 import java.util.TreeSet;
 
 public class TreeSetTest {
     public static void main(String[] args) {
         User u1 = new User(10);
         User u2 = new User(30);
         User u3 = new User(20);
 
         // 默认使用无参构造方法，即比较器为null，此时需要在类中实现Comparable接口的compareTo方法
         TreeSet<User> ts = new TreeSet<>();
         // 也可以自定义一个比较器，传入构造方法
         // TreeSet<User> ts = new TreeSet<>(new UserComparator());
         ts.add(u1);
         ts.add(u2);
         ts.add(u3);
 
         for (User u : ts){
             System.out.println(u);
         }
     }
 }
 
 /*
   如果是需要往TreeSet中添加，则需要实现Comparable接口，并重写compareTo方法
  */
 class User implements Comparable<User>{
     int age;
 
     public User(){
 
     }
 
     public User(int age){
         this.age = age;
     }
 
     /*
       排序会根据compareTo方法返回大于0、小于0或者等于0三种情况进行排序
       可根据自身需要根据三种结果进行自定义排序的规则的编写
      */
     public int compareTo(User u){
         return this.age - u.age;
     }
 
     public String toString(){
         return "User: " + age;
     }
 }
 
 
 /*
   构造器需要实现Comparator接口，并重写compare方法
  */
 class UserComparator implements Comparator<User>{
     public int compare(User u1, User u2){
         return u1.age - u2.age;
     }
 }
```

### 8、HashMap&lt;K,​V&gt;

 **特点：**使用键值对存储数据，key无序不可重复，value是可以重复的，并且是非线程安全的。

**底层原理：**HashMap的底层其实是哈希表或者说散列表的数据结构，而这个哈希表又是由数组和单向链表或者红黑树组成，初始化时它是单向链表，当添加的元素导致单向链表的节点数超过8个时，就会自动转换为红黑树，而红黑树的节点数如果少于了6个，又会自动转换为单向链表，之所以这样实现，还是想要更加快速地对集合元素进行查询。数组部分其实是一个Node类型的一位数组，每个元素都是一个Node对象，Node对象有“final int hash”、“final K key”、“V value”、“Node&lt;K, V&gt; next”四个基本属性，其中hash表示是将key通过从Object重写的hashCode方法返回的哈希值，并且这个hash值在底层可以通过另外的哈希算法（这个算法不需要关注）得到对应的数组下标，即这个hash值可以看成是和数组下标对应的，或者直接将他看成数组下标也行，next则表示下一个节点的内存地址。以最开始的数组和单向链表为例，使用put方法和get方法更加详细的说明HashMap的原理。

**put方法原理：**当执行put方法将一个键值对存入HashMap集合时，会按照以下步骤执行：

* **第一步：**先将key和value封装到Node对象中，然后底层会调用key的hashCode方法（这个方法就是Object类中继承来并重写的方法）得到hash值，此时Node对象中有三个值已经初始化好了：key，value和hash值，另一个next为空。
* **第二步：**将hash值通过底层另外的哈希算法转换为数组的下标，如果数组对应下标位置没有任何元素，就把此Node对象添加到这个位置上。如果下标对应的位置上已经有了元素，那么就会此Node对象的key去和此位置对应的链表从头结点一个一个挨着去使用equals方法进行匹配，如果所有节点的equals方法都返回false，那么就将此Node节点添加到链表的末尾，否则就将equals方法返回true的节点的value覆盖掉。

**get方法原理：**当使用一个key值通过get方法获取对应的value时，会按照以下步骤执行：

* **第一步：**先调用key的hashCode\(\)方法得出hash值，然后通过底层另外的哈希算法得出数组的下标。
* **第二步：**然后通过这个下标快速定位到数组的对应位置上，如果这个位置上什么也没有，则返回null。否则就会使用key在对应位置的单向链表上从头结点开始一个节点一个节点的使用equals方法去比较对应节点的key，如果有一个节点的equals方法返回true，则返回对应的value，否则，如果所有的节点都返回false最后的结果就直接返回null。

**hashCode\(\)和equals\(\)方法重写：**由以上put和get的实现原理可以看出，同一个数组位置的单向链表上的所有节点的hash值都是相同的，但是key的equals方法返回值肯定是不同的。同时也可以发现，使用HashMap时，key对象需要重写两个方法，hashCode\(\)和equals\(\)方法。重写hashCode方法时需要注意以下内容：

* 如果hashCode方法固定返回一个值，那么最终的结果就是只有一个单向链表，这种情况也称之为散列分布不均匀。
* 如果hashCode方法每一个对象都返回不同的值，那么就导致数组的每个位置上都只有一个元素，这肯定也是不行的，变成了单纯的一维数组了，没有单向链表了。
* 重写hashCode时应该尽量保证散列均匀，比如100个对象返回10个hashCode值，这就是散列均匀的。
* **注：**但是不用担心，hashCode\(\)和equals\(\)这两个方法的重写大部分情况都可以通过IDEA工具自动生成，不用我们手动编写。

**数组容量：**HashMap集合的默认容量是16，默认加载因子是0.75，表示默认的数组长度为16，当实际的数组使用达到容量的75%时就会进行自动扩容，扩容之后的容量是原容量的2倍。也可以在初始化时指定容量大小，但是官方推荐初始化容量最好是2的倍数，不然会影响HashMap的执行效率。

**使用示例：**

```java
import java.util.HashMap;
 import java.util.Map;
 import java.util.Set;
 
 
 public class MapTest {
     public static void main(String[] args) {
         Map<Integer, String> map = new HashMap<>();
         map.put(1, "张三");
         map.put(2, "李四");
         // 通过key遍历Map中的键值对
         Set<Integer> keys = map.keySet();
         for (Integer key : keys) {
             String value = map.get(key);
             System.out.println(key + "=" + value);
         }
 
         // 通过Set<Map.Entry<Integer, String>>直接遍历Map中的键值对
         // 如果内存允许，这种方式效率会比较高，适合大数据量的遍历
         Set<Map.Entry<Integer, String>> es = map.entrySet();
         for (Map.Entry<Integer, String> node : es){
             System.out.println(node.getKey() + "=" + node.getValue());
         }
     }
 }
```

### 9、Hashtable&lt;K,​V&gt; 和Properties

Hashtable和HashMap的使用相似的，区别在于Hashtable是线程安全的，而后者是非线程安全的，但是和Vector的情况一样，Hashtable线程安全的实现方式导致了效率较低的问题，所以使用的较少了。

Hashtable的一个子类Properties相比而言要常用一点，Properties也是线程安全的，Properties对象也称之为属性对象，它的特点是key和value都只支持String类型。常用的方法有：

* **Object setProperty\(String key, String value\)：**调用Hashtable的put方法。
* **String getProperty\(String key\)：**获取指定key的value，如果没有则返回null。
* **String getProperty\(String key, String defualtValue\)：**获取指定key的value，如果没有查找到则返回defualtValue。



### 10、TreeMap&lt;K,​V&gt;

**特点：**使用键值对存储数据，key无序不可重复，value是可以重复的，并且存入之后会按key升序自动排序。

**底层原理：**TreeMap实现了SortedMap&lt;K,​V&gt;接口，并且底层其实是自平衡二叉树的数据结构，这种数据结构遵循左小右大的原则存放数据。存入的key和value其实是封装到了一个Entry&lt;K,V&gt;节点对象中，这个节点对象除了存入的key和value，还有三个变量left、right和parent用于存放左子节点、右子节点和父节点的内存地址，这样就刚好形成一个二叉树。遍历这个二叉树时，通常有三种方式：前序遍历（根左右）、中序遍历（左根右）和后序遍历（左右根），这个“前中后”指的是根（节点）在“左右”的位置或者说遍历中的顺序，对于中序遍历，特点是遍历出来的数据自动就是从小到大排序的。



### 11、Collections工具类

这个类专门定义了一些方便集合操作的方法。常用的有：

* **synchronizedList：**将一个非线程安全的列表变为线程安全的。
* **sort：**将一个列表排序。注意，这个接口需要排序的元素实现了Comparable接口，不然会报错。
* **sort\(List集合, 比较器\)：**这种方式就不用实现Comparable接口了，但是需要提供一个比较器。



