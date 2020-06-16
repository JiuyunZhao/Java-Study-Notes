“**java.io.File**”类和文件流的操作没有关系，也就是不能对文件进行读和写，File类是文件和目录路径名的一种抽象表示形式，即一个文件是File对象，一个目录也是File对象，它的构造方法中只需要传入一个文件或目录的路径名即可，注意，对于路径分隔符，Windows和Linux使用的分别是“\”和“/”，但是File中传入的路径可以统一使用“/”，在Windows中也可以识别。

File中的常用方法：

* **boolean exists\(\)：**判断File对象表示的文件或目录是否存在。
* **boolean createNewFile\(\)：**如果File对象不存在，则将File对象创建为一个新的文件，如果存在则不能创建。
* **boolean mkdir\(\)：**如果File对象不存在，则将File对象创建为一个新的目录。注意，此方法只能创建单个目录，不能递归创建目录。
* **boolean mkdirs\(\)：**如果File对象不存在，则将File对象以递归方式创建目录。
* **String getParent\(\)：**获取File对象的父路径（上一级路径）。
* **File getParentFile\(\)：**返回File对象的父路径的File对象。
* **String getAbsolutePath\(\)：**返回File对象的绝对路径。
* **boolean delete\(\)：**删除File对象表示的文件或目录。
* **String getName\(\)：**获取File对象的文件名或目录名。
* **boolean isFile\(\)：**判断File对象是否是一个文件。
* **boolean isDirectory\(\)：**判断File对象是否是一个目录。
* **long lastModified\(\)：**返回File对象最后一次修改的时间的总毫秒数（从1970年1月1日开始）。
* **long length\(\)：**返回File对象的总字节数。
* **boolean renameTo\(File dest\)：**File对象重命名。
* **File\[\] listFiles\(\)：**返回File对象的所有子文件和子目录的File对象数组。



