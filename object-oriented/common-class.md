Java中内置类及其方法的使用通常翻阅对应的API文档即可，但是对于常用的一些类和方法还是需要我们能够熟练的使用。

### 一、System

**System.gc\(\)：**手动启动垃圾回收器，垃圾回收器通常是自动启动的，某些时候Java可能觉得当下的情况并不需要启动gc，但是你又想启动的话，就可以调用这个方法手动启动gc。

**System.exit\(0\)：**终止并退出JVM，exit方法可以传入一个程序结束的状态码，通常传入0就可以了。

**System.currentTimeMillis\(\)：**获取自1970年1月1日 00:00:00 000到当前时间的总毫秒数。

**System.arraycopy​\(Object src, int srcPos, Object dest, int destPos, int length\)：**从源数组src中拷贝若干length连续的元素到目标数组dest当中，src为源数组，srcPos为源数组的起始下标，dest为目标数组，destPos为目标数组的起始下标，length为拷贝的元素个数或长度。

### 二、Object

**protected Object clone\(\)：**返回对象的克隆，注意这个方法的修饰符为protected，只有子类或同一个包下才能使用。

**int hashCode\(\)：**返回对象的哈希值。

**boolean equals\(Object obj\)：**判断两个对象是否相等，默认使用“==”进行判断。注意，“==”的判断其实判断的是变量的值，对于引用类型就是比较的是内存地址，但这通常不是我们想要的结果，因为对于两个对象的比较，我们大多时候认为同一个类的对象，如果它们的属性等对象的内容是相同的话，那么这两个对象也可以认为它们是相同的，而不是去判断那它们的内存地址，所以这种时候就需要重写这个方法了。也由此可以看出，平时判断相等时，如果是基本数据类型则使用“==”进行判断，如果是引用数据类型，如String等，则需要使用它自带的equals方法。

**String toString\(\)：**将对象转换成字符串形式，默认为“类名@对象的内存地址”。在打印某个对象时，也会自动调用对象的这个方法。但是因为这个方法默认的返回值很多时候并不是我们想要的，所以通常都是在需要使用的时候重写它，而不是使用它的默认值。

### 三、String

**方法区的字符串常量池：**字符串虽然也是引用类型的实例对象，但是字符串字面值是存储在方法区中的字符串常量池当中，无论是使用双引号定义还是使用String定义，都会先在方法区内存中创建一个字符串对象，如果以后使用到了相同的字符串，则不会再创建一个字符串对象，而是会直接使用已经存在的字符串对象。

**String类型引用：**需要注意的是，直接使用双引号定义的字符串对象会直接在方法区中直接创建一个字符串对象，但是因为使用new关键字都是会先在堆中创建对应的对象，所以对于使用new关键字创建String对象，如“String s = new String\("hello"\);”，虽然创建的对象在堆中，但s的值（内存地址）其实还是指向的是在方法区的字符串常量池中“"hello"”这个字符串对象。

**字符串垃圾回收：**另外一点，垃圾回收器不会回收字符串常量池中的字符串常量对象。

**常用的构造方法：**

```java
// 最常用的方法
String s1 = "hello";
// 不常用
String s = new String("hello");

// 传入一个byte数组
byte[] bytes = {97, 98, 99};  // 97->a, 98->b, 99->c
String s2 = new String(bytes);
System.out.println(s2);  // 输出：abc
String s3 = new String(bytes, 1, 2);  
System.out.println(s3);  // 输出：bc

// 传入一个char数组
char[] chars = {'h', 'e', 'l', 'l', 'o'};
String s4 = new String(chars);
System.out.println(s4);  // 输出：hello
String s5 = new String(chars, 2, 3);
System.out.println(s5);  // 输出：llo
```

**常用方法：  
**

* **char charAt\(int index\)：**返回指定索引处的char值。
* **int compareTo\(String anotherString\)：**按字符顺序比较两个字符串大小，全部字符都相等则返回0，如果同一个索引位置的字符不相同，就不会再继续比较下去了，并且比较这两个字符，前者大于后者则返回1，前者小于后者则返回-1。
* **boolean contains\(CharSequence s\)：**判读一个字符串中是否包含另一个子串，如果包含则返回true，否则返回false。
* **boolean startsWith\(String prefix\)：**判断一个字符串是否以另一个子串开始，如果是则返回true，否则返回false。
* **boolean endsWith\(String suffix\)：**判断一个字符串是否以另一个子串结尾，如果是则返回true，否则返回false。
* **boolean equals\(Object anObject\)：**判断两个字符串是否相等。注意，应该保持良好的编程习惯，比较两个字符串的相等总应该使用equals方法，而不是使用“==”。
* **boolean equalsIgnoreCase\(String anotherString\)：**比较两个字符串是否相等，忽略大小写。
* **byte\[\] getBytes\(\)：**将一个字符串转换成字节数组并返回。
* **int indexOf\(String str\)：**返回子串在字符串中第一次出现的索引。
* **int lastIndexOf\(String str\)：**返回子串在字符串中最后一次出现的索引。
* **boolean isEmpty\(\)：**判断一个字符串是否为空。
* **int length\(\)：**返回字符串的长度。注意，判断数组长度时，它的length是数组对象的一个属性，而不是方法，字符串对象的length则是方法。
* **String replace\(char oldChar, char newChar\)：**返回一个新的字符串，原字符串中所有的oldChar都将被替换为newChar。
* **String replace\(CharSequence target, CharSequence replacement\)：**返回一个新的字符串，原字符串中的所有target都将被替换为replacement。注意String的父接口就是CharSequence，所以这里就传入String对象即可。
* **String\[\] split\(String regex\)：**返回一个根据指定正则表达式（分隔符）切割后的字符串数组。
* **String substring\(int beginIndex\)：**返回一个从指定索引位置往后截取的子串。
* **String substring\(int beginIndex, int endIndex\)：**返回一个从指定开始索引到结束索引（endIndex-1）的子串，注意，返回的子串不包括endIndex处的字符。
* **char\[\] toCharArray\(\)：**返回一个将字符串转换为字符的数组。
* **String toLowerCase\(\)：**返回一个将字符串全部字符都转换为小写的字符串。
* **String toUpperCase\(\)：**返回一个将字符串全部字符都转换为大写的字符串。
* **String trim\(\)：**返回一个去处字符串前后空白的字符串。
* **String valueOf\(\)：**将另一个不是String的对象转换为字符串对象。注意这是一个静态方法，即使用的时候应该这样“String s = String.valueOf\(111\);”。

### 四、StringBuffer/StringBuilder

如果需要进行大量的字符串拼接操作，建议使用Java自带的StringBuffer或StringBuilder，而不是使用加号+。

**StringBuffer示例：**

```java
public class Test{
    public static void main(String[] args){
        // 默认会创建一个长度为16的byte[]
        // StringBuffer stringBuffer = new StringBuffer();
        // 创建一个指定长度的StringBuffer
        StringBuffer stringBuffer = new StringBuffer(100);

        // 拼接字符串统一调append方法，当拼接的字符串长度超过了16或者指定的容量，这个byte[]数组会自动扩容
        stringBuffer.append("abc");
        stringBuffer.append(3.14);
        stringBuffer.append(true);
    }
}
```

**关于StringBuffer的优化：**其中一个优化点是在初始化时预估一下需要的字符串长度，给一个大一点的长度（默认的数组只有16个byte），以减少后续数组扩容的次数，关键点在于这个长度不能太大也不能太小，太大也浪费空间，太小会增加扩容次数。

**StringBuilder和StringBuffer的区别：**两者在示例的使用上都一样的，都可以用于字符串的拼接，但区别在于StringBuffer是有synchronized关键字修饰的，表示在多线程的环境下是线程安全的，StringBuilder则没有synchronized关键字修饰，表示不是线程安全的。

**对比String：**String的底层其实是一个final类型的byte\[\]， StringBuffer底层同样是一个byte\[\]，但区别是没有final修饰符，因为数组的长度一旦确定了就不能改变，这表示final修饰的String类型的变量（引用）指向的内存地址是不能改变的，即该字符串是不能改变的，但是如果没有final修饰符，则这个数组满了之后可以扩容，StringBuffer的变量就可以指向扩容后新的数组的内存地址。说到String的这个final，特别需要注意的是，对于如“String s = "hello";”而言，final修饰的是“"hello"”这个字符串对象本身，而不是变量s（引用），s是可以重新给它赋值的。

### 五、8中基本数据类型的包装类

包装类之所以存在，就是为了应对一种情况，一些方法的参数需要是Object类型，但是又需要传入Java中的基本数据类型，基本数据类型因为类型不兼容所以肯定是不能传进去的，此时就需要使用基本数据类型经过包装过后的的包装类了，包装类型都属于引用数据类型，且它们的最终父类都是Object类，此时就可以传入这些方法中进行使用了。

**基本数据类型对应的包装类：**除了int和char类型对应的包装类略有不同外，其他类型的包装类都是其本身单词的首字母大写形式。

* **byte：**Byte
* **short：**Short
* **int：**Integer
* **long：**Long
* **float：**Float
* **double：**Double
* **boolean：**Boolean
* **char：**Character

**装箱：**将基本数据类型转换为引用数据类型（对应包装类）称之为装箱。

**拆箱：**将包装类对象转换为基本数据类型（不一定是原来的基本数据类型了）称之为拆箱。

**手动装箱和拆箱示例：**

```java
public class Test{
    public static void main(String[] args){
        // 装箱：将基本数据类型转换为引用数据类型
        Integer i  = new Integer(100);
        // 拆箱：将包装类对象转换为基本数据类型
        float f = i.floatValue();
        // 拆箱：将包装类对象转换为基本数据类型
        int iv = i.intValue();
    }
}
```

**自动装箱和自动拆箱：**平时使用的时候都是使用的自动转换，上面示例的方式一般不常用了。

```java
public class Test{
    public static void main(String[] args){
        // 自动装箱：自动将基本数据类型转换为引用数据类型
        Integer i = 100;

        // 自动拆箱：自动将包装类对象转换为基本数据类型
        int iv = i;

        // 对于算术运算，会进行自动拆箱操作
        System.out.println(i + 1);
    }
}
```

**整数型常量池：**Java中为了提高程序的执行效率，将\[-128, 127\]之间的所有包装对象提前创建好了，并放到了方法区中的整数型常量池中，以后只要用到这些对象就不会再去new对象了，就像字符串常量池一样。

**Integer常用方法：**因为包装类的用法大部分都相同，不同的部分也可以通过查阅帮助文档来了解使用，所以这里只展示Integer类的常用方法。

* **java.lang.NumberFormatException异常：**如“Integer a = new Integer\("哈喽"\);”，这个语句在编译时不会报错，因为Integer构造方法允许传入一个字符串，但是“哈喽”这个字符串明显不是数字，执行时就会报数字格式化异常。
* **static int parseInt\(String s\)：**这是一个静态方法，将一个字符串转换为一个int类型的数字，这个方法同样不能传入一个不是数字的字符串，其他包装类也有比如parseDouble、parseFloat等转换作用的方法。
* **static String toBinaryString\(int i\)：**将十进制的整数转换为二进制的字符串。
* **static String toHexString\(int i\)：**将十进制的整数转换为十六进制的字符串。
* **static String toOctalString\(int i\)：**将十进制的整数转换为八进制的字符串。

### 六、Date/SimpleDateFormat

Java中日期的处理使用“java.util.Date”类，对日期进行格式化则使用“java.text.SimpleDateFormat”类。

**使用示例：**

```java
// 专门进行日期格式化的类
import java.text.SimpleDateFormat;
// 日期操作的类
import java.util.Date;


public class DateTest {
    public static void main(String[] args) throws Exception {
        // 直接调用无参构造方法，会默认使用当前系统日期时间来进行初始化，精确到毫秒
        Date nowTime = new Date();
        // 输出当前日期时间
        System.out.println(nowTime);

        // 按照指定格式创建日期格式化对象
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        // 将日期对象格式化为指定格式的字符串
        String nowTimeStr = sdf.format(nowTime);
        System.out.println(nowTimeStr);

        // 将时间字符串按格式转换为Date对象，需要注意，指定的格式需要和字符串中的格式相同
        String t = "2020-06-01 21:18:22 333";
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        Date dateTime = sdf2.parse(t);

        // 获取自1970年1月1日 00:00:00 000到当前日期的总毫秒数
        long nowTimeMillis = System.currentTimeMillis();
        System.out.println(nowTimeMillis);

        // 根据毫秒数创建一个Date对象
        Date tt = new Date(nowTimeMillis);
    }
}
```

**日期时间格式化字符如下：**

| **日期时间格式化字符** |
| :---: |


| Letter | Date or Time Component | Presentation | Examples |
| :--- | :--- | :--- | :--- |
| `G` | Era designator | [Text](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#text) | `AD` |
| `y` | Year | [Year](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#year) | `1996`; `96` |
| `Y` | Week year | [Year](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#year) | `2009`; `09` |
| `M` | Month in year \(context sensitive\) | [Month](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#month) | `July`; `Jul`; `07` |
| `L` | Month in year \(standalone form\) | [Month](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#month) | `July`; `Jul`; `07` |
| `w` | Week in year | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `27` |
| `W` | Week in month | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `2` |
| `D` | Day in year | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `189` |
| `d` | Day in month | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `10` |
| `F` | Day of week in month | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `2` |
| `E` | Day name in week | [Text](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#text) | `Tuesday`; `Tue` |
| `u` | Day number of week \(1 = Monday, ..., 7 = Sunday\) | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `1` |
| `a` | Am/pm marker | [Text](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#text) | `PM` |
| `H` | Hour in day \(0-23\) | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `0` |
| `k` | Hour in day \(1-24\) | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `24` |
| `K` | Hour in am/pm \(0-11\) | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `0` |
| `h` | Hour in am/pm \(1-12\) | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `12` |
| `m` | Minute in hour | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `30` |
| `s` | Second in minute | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `55` |
| `S` | Millisecond | [Number](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#number) | `978` |
| `z` | Time zone | [General time zone](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#timezone) | `Pacific Standard Time`; `PST`; `GMT-08:00` |
| `Z` | Time zone | [RFC 822 time zone](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#rfc822timezone) | `-0800` |
| `X` | Time zone | [ISO 8601 time zone](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/text/SimpleDateFormat.html#iso8601timezone) | `-08`; `-0800`; `-08:00` |

### 七、数字格式化

数字格式化应该使用“java.text.DecimalFormat”，如果处理大数据或者财务数据，应该使用“java.math.BigDecimal”，因为它的精度极高，具体使用可参阅API文档。

**部分格式化字符：**

* **\#：**井号表示任意字符。
* **,：**逗号表示千分位。
* **.：**点号表示小数点。
* **0：**0表示不足的位数用0补齐。

```java
import java.text.DecimalFormat;

public class DecimalFormatTest{
    public static void main(String[] args){
        DecimalFormat df = new DecimalFormat("###,###.##");
        String s = df.format(1234.561);
        System.out.println(s);  // 输出：1,234.56

        DecimalFormat df2 = new DecimalFormat("###,###.0000");
        String s2 = df2.format(1234.56);
        System.out.println(s2);  // 输出：1,234.5600
    }
}
```

### 八、随机数

想要生成一个随机数，应该使用“java.util.Random”，详细用法请参阅API文档。

```java
import java.util.Random;

public class RandomTest{
    public static void main(String[] args){
        Random random = new Random();
        // 生成一个随机整数
        int num1 = random.nextInt();

        // 生成一个[0, 100]的随机整数，注意，这里不包括101
        int num2 = random.nextInt(101);
    }
}
```

### 九、枚举

枚举类型，关键字enum，也是一种引用数据类型，一个枚举编译后也会生成一个“.class”字节码文件。

枚举中的每一个值可以看做是常量，且每个枚举值只需要定义对应的名称即可，不需要给它赋值。

```java
public enum Result{
    // 只需要定义每个枚举项的名称即可，不需要给它赋值。
    // 如果只有两个枚举项，建议使用boolean类型即可，多余两个时才建议使用枚举类型。
    SUCCESS, FAIL
}
```

```java
public class Test{
    public static void main(String[] args){
        Result res = divide(3, 0);
        System.out.println(res);
    }

    public static Result divide(int a, int b){
        try{
            int c = a / b;
            return Result.SUCCESS;
        } catch (Exception e) {
            return Result.FAIL;
        }
    }
}
```



