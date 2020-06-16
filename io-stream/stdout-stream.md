标准输出不需要手动close\(\)关闭。

**PrintStream：**标准的字节输出流，默认输出到控制台。System.out其实就是一个PrintStream标准输出字节流，可以使用System.setOut设置标准输出流输出到指定指定文件，不再默认输出到控制台。

**PrintWriter：**标准的字符输出流，默认输出到控制台，同样可以使用System.setOut更改默认输出方向。

**使用示例：**

```java
PrintStream out = new PrintStream(new FileOutputStreasm("log.txt", true));
System.setOut(out);
```



