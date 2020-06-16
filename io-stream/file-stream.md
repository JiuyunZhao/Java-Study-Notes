### FileInputStream（常用，重点掌握）

对文件以字节流的方式进行读取，构造方法可以传入一个文件路径，也可以传入一个表示文件的File对象。

常用的方法有：

* **int read\(\)：**从文件开始每次往后读取一个字节，注意返回的是字节本身，并没有将字节转换为对应的字符，例如第一个字节的字符是“a”，读取时则返回的是97。如果到了文件末尾则返回-1。注意文件指针一开始并没有指向文件的第一个字节，而是在第一次调用read\(\)方法时才指向并返回文件的第一个字节，之后每次调用read\(\)方法就会往后“挪”一个字节。但是读取文件内容时这个方法并不常用，因为每次只读取一个字节的方式导致和硬盘的交互太频繁了。
* **int read\(byte\[\] b\)：**传入一个byte数组，每次读取数组的length个字节，将读取的内容覆盖数组原有的值，并返回读取的字节数；如果文件剩余字节数不足length，那么就会取出全部剩余字节，并将数组从头开始覆盖，而数组剩余的部分不会做任何操作；当文件指针已处在末尾了，将返回-1，数组同样不会做任何操作。
* **int available\(\)：**返回流当中剩余的没有读到的字节数。可以在读之前使用这个方法查看文件的总字节数。
* **long skip\(long n\)：**跳过指定数量的字节（不去读）。

**使用示例：**

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileInputStreamTest {
    public static void main(String[] args) {
        FileInputStream fis = null;
        try{
            // FileInputStream中抛出了一个FileNotFoundException异常，即编译时异常
            // 需要程序员手动处理，这里加一个try进行捕获
            // input.txt文件中只有一行数据：helloworld
            fis = new FileInputStream("Z:\\Study\\Java\\input.txt");

            /*
            // 以下read()只是演示，实际并不常用
            // 打印结果为：
            // 104
            // 101
            // 108
            // 108
            // 111
            // 119
            // 111
            // 114
            // 108
            // 100
            int byteData = 0;
            while ((byteData = fis.read()) != -1) {
                System.out.println(byteData);
            }
            */


            // 以下使用read(byte[] b)方法读取文件内容
            byte[] bytes = new byte[6];
            int count = fis.read(bytes);
            System.out.println(count);  // 6
            System.out.println(new String(bytes));  // hellow
            System.out.println(new String(bytes, 0, count));  // hellow

            count = fis.read(bytes);
            System.out.println(count);  // 4
            System.out.println(new String(bytes));  // orldow
            System.out.println(new String(bytes, 0, count));  // orld

            count = fis.read(bytes);
            System.out.println(count);  // -1
            System.out.println(new String(bytes));  // orldow

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    // close方法也是抛出了一个IOException异常的编译时异常，同样加一个try进行捕获
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



### FileOutpuStream（常用，重点掌握）

将字节内容写入到文件中，构造方法中同样可以传入一个文件路径或者表示文件的File对象，同时还可以指定第二参数为true。当只传入一个文件路径或File对象，如果文件已存在，会清空文件内容，如果文件不存在，则会自动创建该文件；也可以指定第二个参数为true，表示如果文件已存在则不会清空文件，而是会往文件末尾追加数据。

常用的方法有：

* **void write\(byte\[\] b\)：**将byte数组中的全部内容写入到文件中。
* **void write\(byte\[\] b, int off, int len\)：**将byte数组中的部分内容写入到文件中。
* **输出字符串：**可以将字符串通过自身的getBytes\(\)转换为byte数组之后再进行输出。

**使用示例：**

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamTest {
    public static void main(String[] args) {
        FileOutputStream fos = null;
        try {
            // 只传入一个文件路径：如果文件已存在，会清空文件内容，如果文件不存在，则会自动创建该文件
            // fos = new FileOutputStream("Z:\\Study\\Java\\output.txt");

            // 第二个参数表示如果文件已存在则不会清空文件，而是会往文件末尾追加数据
            fos = new FileOutputStream("Z:\\Study\\Java\\output.txt", true);

            byte[] bytes = {97, 98, 99, 100};
            // 将bytes数组全部写到文件中
            fos.write(bytes);
            // 将bytes数组的一部分写到文件中
            fos.write(bytes, 0, 2);

            // 将字符串输出到文件中
            String s = "你好，世界";
            byte[] bs = s.getBytes();
            fos.write(bs);

            // 文件输出完成后，一定记得刷新一下
            fos.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    // 文件操作完成后一定记得调用close()方法
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### FileReader

和FileInputStream相比，在使用上都是类似的，只是需要把byte数组换成char数组即可。



### FileWriter

和FileOutpuStream相比，在使用上也都是类似的，只是要把byte数组换成char数组，另外，由于字符串的特殊性，FileWriter的write方法还可以直接写入一个字符串到文件。



