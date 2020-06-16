如果一个文本文件中的每一行数据是像这样“key=value”使用等号或冒号（冒号不建议）分隔成两部分，就可以使用一个文件输入流读取出来，再使用Properties对象的load方法加载这个文件流输入对象，然后在Properties的getProperty方法传入等号左边的字符串就可以取出等号右边的字符串了。

user-conf.txt

```
username=root
password=123456
```

```java
 FileReader reader = new FileReader("user-conf.txt");
 Properties pro = new Properties();
 pro.load(reader);
 String username = pro.getProperty("username");
 System.out.println(username);  // root
```



