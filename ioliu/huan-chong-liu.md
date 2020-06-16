缓冲流表示带有缓冲区的字节流或字符流，特点是使用这些流的时候不需要自定义byte数组或char数组。

**包装流和节点流：**缓冲流的使用涉及到一些概念，当一个流的构造方法中需要另一个流的时候，被传入的这个流称为节点流，外部负责包装这个节点流的流称为包装流或处理流。关闭包装流的时候也是只需要调用最外层包装流的close\(\)方法即可，里面的节点流会自动关闭。同样，对于输出的流，也是只需要调用最外层包装流的flush\(\)方法即可。

**BufferedInputStream：**是一个包装流，构造方法中需要传入一个InputStream类型（字节类型）的节点流。

**BufferedOutputStream：**是一个包装流，构造方法中需要传入一个OutputStream类型（字节类型）的节点流。

**BufferedReader：**是一个包装流，构造方法中需要传入一个Reader类型（字符类型）的节点流。

**BufferedWriter：**是一个包装流，构造方法中需要传入一个Writer类型（字符类型）的节点流。

**使用示例：**

```java
import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderTest {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            // 这里reader就是一个节点流，br就是一个包装流或处理流
            // 文件内容只有一行：helloworld
            FileReader reader = new FileReader("Z:\\Study\\Java\\input.txt");
            br = new BufferedReader(reader);

            // 读取一行数据，注意返回的数据是不包含末尾的换行符的
            String line = br.readLine();
            System.out.println(line);  // helloworld

            // 读取到文件末尾，返回null
            line = br.readLine();
            System.out.println(line);  // null

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 对于包装流来说，只需要调用最外层包装流的close()方法即可，里面的节点流会自动关闭。
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



