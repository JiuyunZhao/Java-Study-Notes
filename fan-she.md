### 1. 反射机制

反射机制的相关类除了一个java.lang.Class，其余都在java.lang.reflect包下。

反射机制用于读取class字节码文件，需要注意，JVM加载字节码到内存中时都只会保存一份，多次读取class文件时不用担心也会加载多次。

反射机制相关的常用类：

* **java.lang.Class：**代表整个类的字节码，表示一个类型。
* **java.lang.reflect.Method：**代表字节码中的方法字节码，表示一个方法。
* **java.lang.reflect.Constructor：**代表字节码中的构造方法字节码，表示一个构造方法。
* **java.lang.reflect.Field：**代表字节码中的属性字节码，表示一个属性。

 

### 2. 反射类字节码Class（类/类型）

获取类的字节码（java.lang.Class类）有三种方式：

* **第一种方式：**通过Class类的静态方法forName，例如Class c1 = Class.forName\("java.lang.String"\);就表示获取到了String这个类的class字节码，注意，这是Class类下的一个静态方法，参数需要是完整的包名。另外，Class.forName方法的使用会导致类的加载，也就是说如果希望只是执行一个类的静态代码块，并不执行其他的代码，就可以使用这个方法来进行类的加载，此时，自然就会去执行对应的静态代码了。
* **第二种方式：**通过Object类的getClass方法，例如String s = "abc"; Class c2 = s.getClass\(\);，即通过任何类对象的getClass方法就可以拿到对应了类字节码了，并且因为JVM只会在内存中加载一份相同类的字节码，所以这个例子的c2和第一种方式的c1使用双等号判断返回结果是true。
* **第三种方式：**通过类的class属性，例如Class c3 = String.class;，java中任何类型都有class属性。

Class中常用的方法：

* **String getName\(\)：**返回类的完整类名（包含包路径）。
* **String getsimpleName\(\)：**返回类的简类名（类定义名称）。
* **Field\[\] getFields\(\)：**获取Class中所有public类型的Field对象（属性）。
* **Field\[\] getDeclaredFields\(\)：**获取Class中所有的Field对象（属性）。
* **Field getDeclaredField\(String name\)：**获取指定名称的Field对象（属性）。
* **Method\[\] getDeclaredMethods\(\)：**获取所有的Method对象（方法）。
* **Method getDeclaredMethod\(String name, Class&lt;?&gt;... parameterTypes\)：**根据方法名称和参数类型列表获取Method对象（方法），例如“userClass.getDeclaredMethod\("login", String.class, int.class\);”。
* **Constructor&lt;?&gt;\[\] getDeclaredConstructors\(\)：**获取所有的Constructor对象（构造方法）。
* **Constructor&lt;T&gt; getDeclaredConstructor\(Class&lt;?&gt;... parameterTypes\)：**获取指定参数类型列表的Constructor对象（构造方法），例如“userClass.getDeclaredConstructor\(String.class, int.class\);”。
* **Class&lt;? super T&gt; getSuperclass\(\)：**获取类的父类。
* **Class&lt;?&gt;\[\] getInterfaces\(\)：**获取所有类实现的接口。

 

### 3. 反射属性字节码Field（字段/属性）

获取Field需要先获取到对应的类字节码Class，然后才能从类中获取到对应的Field（java.lang.reflect.Field）。

Field常用方法：

* **Class&lt;?&gt; getType\(\)：**返回属性的数据类型。
* **String getName\(\)：**返回属性的名称。
* **int getModifiers\(\)：**返回修饰符列表的代号，此代号可以使用java.lang.reflect.Modifier的静态方法toString方法传入代号获取到具体的修饰符名称列表。
* **void set\(Object obj, Object value\)：**给指定对象的Field对象赋予值value。
* **Object get\(Object obj\)：**获取指定对象的Field值（属性值）。
* **void setAccessible\(boolean flag\)：**设置为true时表示打破封装，如果不调用这个方法设置为true的话就没办法获取对象的私有属性了，调用这个方法之后就可以获取到所有的属性了，包括私有属性。

 

### 4. 反射方法字节码Method（方法）

获取Method需要先获取到对应的类字节码Class，然后才能从类中获取到对应的Method（java.lang.reflect.Method）。

Method中的常用方法：

* **Class&lt;?&gt; getReturnType\(\)：**获取返回值的数据类型。
* **Class&lt;?&gt;\[\] getParameterTypes\(\)：**获取方法的参数类型列表。
* **int getModifiers\(\)：**返回修饰符列表的代号，此代号可以使用java.lang.reflect.Modifier的静态方法toString方法传入代号获取到**具体的修饰符名称列表。  **
* **Object invoke\(Object obj, Object... args\)：**调用指定对象的方法。

 

### 5. 反射构造方法Constructor（构造方法）

获取Constructor需要先获取到对应的类字节码Class，然后才能从类中获取到对应的Method（java.lang.reflect.Constructor）。

Constructor中的常用方法：

* **int getModifiers\(\)：**返回修饰符列表的代号，此代号可以使用java.lang.reflect.Modifier的静态方法toString方法传入代号获取到具体的修饰符名称列表。
* **T newInstance\(Object... initargs\)：**使用newInstance方法创建一个实例对象。



