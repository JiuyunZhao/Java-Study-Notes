### 1、if语句

```java
// 第一种
 if(布尔表达式){
     java 语句;
     ...
 }

 // 第二种
 if(布尔表达式){
     java 语句;
     ...
 }else{
     java 语句;
     ...
 }

 // 第三种
 if(布尔表达式){
     java 语句;
     ...
 }else if{
     java 语句;
     ...
 }else if{
 ...
 }

 // 第四种
 if(布尔表达式){
     java 语句;
     ...
 }else if{
     java 语句;
     ...
 }else if{
 ...
 }else{
     java 语句;
     ...
 }
```

注意，如果大括号中如果只有一条Java语句，那么这个大括号是可以不用写的（不推荐），但同时也就意味着如果这个if语句之后没有大括号，却有多个Java语句，那么只有第一个Java语句会被作为if语句的条件执行语句，其他的语句则是与if语句平行的语句。

### 2、switch语句

```java
// 正常语法
 switch(int或String类型的字面值或变量){
     case int或String类型的字面值或变量:
         java语句;
         ...
         break;
     case int或String类型的字面值或变量:
         java语句;
         ...
         break;
     ...
     default:
         java语句;
         ...
 }
 // case合并语法
 switch(int或String类型的字面值或变量){
     // 直接将多个case写在一起即可
     case int或String类型的字面值或变量:case int或String类型的字面值或变量:
         java语句;
         ...
         break;
     case int或String类型的字面值或变量:
         java语句;
         ...
         break;
     ...
     default:
         java语句;
         ...
 }
```

**步骤和原理：**将switch后括号中的变量的值与case后的值匹配，如果两个值相等，则执行对应case中的语句，如果都没匹配上，则执行default中的语句，case中的break语句表示结束switch语句，case中没有break语句的话，就会继续执行之后所有没有break语句的case中的内容，并且之后的这些case并不会进行匹配后再判断是否执行，而是直接执行case的语句，直到遇到break语句退出switch语句，这种现象也称之为case穿透。

switch中的数据类型也可以是byte、short和char，因为它们都可以自动转换为int类型，注意，在switch和case处如果不是int或String类型，最终都是先转换成int或String后再进行匹配判断。

### 3、for循环

```java
 // 语法
 for(初始化表达式; 布尔表达式; 更新表达式){
     // 需要重复执行的代码片段，也称为循环体
     Java语句;
 }
```

注意，初始化表达式、布尔表达式、更新表达式都不是必须的，但是括号中的两个分号是必须的，不能少。

**执行步骤：  
**

1. 先执行初始化表达式，并且该表达式只执行一次。
2. 判断布尔表达式的结果是true还是false，如果是false，则退出循环；如果是true，则执行循环体。
3. 执行更新表达式式。
4. 重新开始第二步。

**foreach语法：**

foreach语法是普通for循环的一个增强版本，在代码编写上也简便了许多，但是因为不能使用下标，所以还是需要根据实际情况来使用。

```java
 // 语法：for(元素类型 变量名 : 数组或集合){循环体}
 int[] array = {1, 2, 3, 4};
 for (int e : array){
     System.out.println(e);
 }
```

### 4、while循环和dowhile循环

```java
// 语法
 while(布尔表达式){
     循环体;
 }
```

**执行步骤：**判断布尔表达式的结果，如果结果为true则执行循环体，否则退出循环，然后反复执行这一步骤。

```java
 // 语法
 do{
     循环体;
 }while(布尔表达式); // 这里的分号不要忘记了
```

**特点：**dowhile结构的特点是循环体至少会执行一次。

### 5、break和continue

break用于终止循环，终止其当前所在的循环体。

continue用于在循环体中跳过本次循环，不再执行continue之后的语句，直接进入下一次循环的执行。

