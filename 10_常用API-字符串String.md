# 10_字符串类String

## 1. 字符串类String

### 1.String概述

​	String 类代表字符串，Java 程序中的所有字符串文字（例如“abc”）都被实现为此类的实例。也就是说，Java 程序中所有的双引号字符串，都是 String 类的对象。String 类在 java.lang 包下，所以使用的时候不需要导包！

### 2.String类的特点

- 字符串不可变，它们的值在创建后不能被更改
- 虽然 String 的值是不可变的，但是它们可以被共享
- 字符串效果上相当于字符数组( char[] )，但是底层原理是字节数组( byte[] )

### 3.字符串对象的创建

1. 方式1 ： 最简单的方式

   ```java
   String str = "你好";
   ```

2. 方式2：构造函数方式

   ```java
   String str = new String("字符串");//这种方式不会进入缓冲池
   ```

3. 创建字符串对象两种方式的区别

   - 通过构造方法创建

     ​	通过 new 创建的字符串对象，每一次 new 都会申请一个内存空间，虽然内容相同，但是地址值不同

   - 直接赋值方式创建
   
     ​	以“”方式给出的字符串，只要字符序列相同(顺序和大小写)，无论在程序代码中出现几次，JVM 都只会建立一个 String 对象，并在字符串池中维护
   
4. 常用构造方法

   | 方法名                      | 说明                                      |
   | --------------------------- | ----------------------------------------- |
   | public   String()           | 创建一个空白字符串对象，不含有任何内容    |
   | public   String(char[] chs) | 根据字符数组的内容，来创建字符串对象      |
   | public   String(byte[] bys) | 根据字节数组的内容，来创建字符串对象      |
   | String s =   “abc”;         | 直接赋值的方式创建字符串对象，内容就是abc |

5. 示例代码

   ```java
   public class StringDemo01 {
       public static void main(String[] args) {
           //public String()：创建一个空白字符串对象，不含有任何内容
           String s1 = new String();
           System.out.println("s1:" + s1);
   
           //public String(char[] chs)：根据字符数组的内容，来创建字符串对象
           char[] chs = {'a', 'b', 'c'};
           String s2 = new String(chs);
           System.out.println("s2:" + s2);
   
           //public String(byte[] bys)：根据字节数组的内容，来创建字符串对象
           byte[] bys = {97, 98, 99};
           String s3 = new String(bys);
           System.out.println("s3:" + s3);
   
           //String s = “abc”;	直接赋值的方式创建字符串对象，内容就是abc
           String s4 = "abc";
           System.out.println("s4:" + s4);
       }
   }
   ```

## 2.String类API

- - | `char`          | `charAt(int index)`                                          | 返回指定索引处的 `char`值。                                  |
    | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | `IntStream`     | `chars()`                                                    | 返回一个 `int`的流，从这个序列中扩展 `char`值。              |
    | `int`           | `codePointAt(int index)`                                     | 返回指定索引处的字符（Unicode代码点）。                      |
    | `int`           | `codePointBefore(int index)`                                 | 返回指定索引之前的字符（Unicode代码点）。                    |
    | `int`           | `codePointCount(int beginIndex,  int endIndex)`              | 返回此 `String`的指定文本范围内的Unicode代码点数。           |
    | `IntStream`     | `codePoints()`                                               | 从此序列返回码流值。                                         |
    | `int`           | `compareTo(String anotherString)`                            | 按字典顺序比较两个字符串。                                   |
    | `int`           | `compareToIgnoreCase(String str)`                            | 按字典顺序比较两个字符串，忽略病例差异。                     |
    | `String`        | `concat(String str)`                                         | 将指定的字符串连接到该字符串的末尾。                         |
    | `boolean`       | `contains(CharSequence s)`                                   | 当且仅当此字符串包含指定的char值序列时才返回true。           |
    | `boolean`       | `contentEquals(CharSequence cs)`                             | 将此字符串与指定的 `CharSequence` 。                         |
    | `boolean`       | `contentEquals(StringBuffer sb)`                             | 将此字符串与指定的 `StringBuffer` 。                         |
    | `static String` | `copyValueOf(char[] data)`                                   | 相当于 `valueOf(char[])`。                                   |
    | `static String` | `copyValueOf(char[] data,  int offset, int count)`           | 相当于 `valueOf(char[],  int, int)`                          |
    | `boolean`       | `endsWith(String suffix)`                                    | 测试此字符串是否以指定的后缀结尾。                           |
    | `boolean`       | `equals(Object anObject)`                                    | 将此字符串与指定对象进行比较。                               |
    | `boolean`       | `equalsIgnoreCase(String anotherString)`                     | 将此 `String`与另一个 `String`比较，忽略案例注意事项。       |
    | `static String` | `format(String format,  Object... args)`                     | 使用指定的格式字符串和参数返回格式化的字符串。               |
    | `static String` | `format(Locale l, String format,  Object... args)`           | 使用指定的区域设置，格式字符串和参数返回格式化的字符串。     |
    | `byte[]`        | `getBytes()`                                                 | 使用平台的默认字符集将该 `String`编码为一系列字节，将结果存储到新的字节数组中。 |
    | `byte[]`        | `getBytes(String charsetName)`                               | 使用命名的字符集将该 `String`编码为一系列字节，将结果存储到新的字节数组中。 |
    | `byte[]`        | `getBytes(Charset charset)`                                  | 使用给定的[charset](../../java/nio/charset/Charset.html)将该`String`编码为字节序列，将结果存储到新的字节数组中。 |
    | `void`          | `getChars(int srcBegin,  int srcEnd, char[] dst, int dstBegin)` | 将此字符串中的字符复制到目标字符数组中。                     |
    | `int`           | `hashCode()`                                                 | 返回此字符串的哈希码。                                       |
    | `int`           | `indexOf(int ch)`                                            | 返回指定字符第一次出现的字符串内的索引。                     |
    | `int`           | `indexOf(int ch,  int fromIndex)`                            | 返回指定字符第一次出现的字符串内的索引，以指定的索引开始搜索。 |
    | `int`           | `indexOf(String str)`                                        | 返回指定子字符串第一次出现的字符串内的索引。                 |
    | `int`           | `indexOf(String str,  int fromIndex)`                        | 返回指定子串的第一次出现的字符串中的索引，从指定的索引开始。 |
    | `String`        | `intern()`                                                   | 返回字符串对象的规范表示。                                   |
    | `boolean`       | `isEmpty()`                                                  | 返回 `true`如果，且仅当 [`length()`](../../java/lang/String.html#length--)为  `0` 。 |
    | `static String` | `join(CharSequence delimiter, CharSequence... elements)`     | 返回一个由连接在一起的 `CharSequence elements`副本组成的新字符串，并附带指定的  `delimiter`的副本。 |
    | `static String` | `join(CharSequence delimiter, Iterable<? extends CharSequence> elements)` | 返回一个新 `String`的副本组成 `CharSequence  elements`与指定的副本一起加入 `delimiter` 。 |
    | `int`           | `lastIndexOf(int ch)`                                        | 返回指定字符的最后一次出现的字符串中的索引。                 |
    | `int`           | `lastIndexOf(int ch,  int fromIndex)`                        | 返回指定字符的最后一次出现的字符串中的索引，从指定的索引开始向后搜索。 |
    | `int`           | `lastIndexOf(String str)`                                    | 返回指定子字符串最后一次出现的字符串中的索引。               |
    | `int`           | `lastIndexOf(String str,  int fromIndex)`                    | 返回指定子字符串的最后一次出现的字符串中的索引，从指定索引开始向后搜索。 |
    | `int`           | `length()`                                                   | 返回此字符串的长度。                                         |
    | `boolean`       | `matches(String regex)`                                      | 告诉这个字符串是否匹配给定的 [regular  expression](../util/regex/Pattern.html#sum) 。 |
    | `int`           | `offsetByCodePoints(int index,  int codePointOffset)`        | 返回这个 `String`中的索引，它与给定的 `index`由  `codePointOffset`代码点偏移。 |
    | `boolean`       | `regionMatches(boolean ignoreCase,  int toffset, String other, int ooffset,  int len)` | 测试两个字符串区域是否相等。                                 |
    | `boolean`       | `regionMatches(int toffset,  String other, int ooffset,  int len)` | 测试两个字符串区域是否相等。                                 |
    | `String`        | `replace(char oldChar,  char newChar)`                       | 返回从替换所有出现的导致一个字符串 `oldChar` ，在这个字符串  `newChar` 。 |
    | `String`        | `replace(CharSequence target, CharSequence replacement)`     | 将与字面目标序列匹配的字符串的每个子字符串替换为指定的文字替换序列。 |
    | `String`        | `replaceAll(String regex,  String replacement)`              | 用给定的替换替换与给定的 [regular  expression](../util/regex/Pattern.html#sum)匹配的此字符串的每个子字符串。 |
    | `String`        | `replaceFirst(String regex,  String replacement)`            | 用给定的替换替换与给定的 [regular  expression](../util/regex/Pattern.html#sum)匹配的此字符串的第一个子字符串。 |
    | `String[]`      | `split(String regex)`                                        | 将此字符串拆分为给定的 [regular  expression的](../util/regex/Pattern.html#sum)匹配。 |
    | `String[]`      | `split(String regex,  int limit)`                            | 将此字符串拆分为给定的 [regular  expression的](../util/regex/Pattern.html#sum)匹配。 |
    | `boolean`       | `startsWith(String prefix)`                                  | 测试此字符串是否以指定的前缀开头。                           |
    | `boolean`       | `startsWith(String prefix,  int toffset)`                    | 测试在指定索引处开始的此字符串的子字符串是否以指定的前缀开头。 |
    | `CharSequence`  | `subSequence(int beginIndex,  int endIndex)`                 | 返回一个字符序列，该序列是该序列的子序列。                   |
    | `String`        | `substring(int beginIndex)`                                  | 返回一个字符串，该字符串是此字符串的子字符串。               |
    | `String`        | `substring(int beginIndex,  int endIndex)`                   | 返回一个字符串，该字符串是此字符串的子字符串。               |
    | `char[]`        | `toCharArray()`                                              | 将此字符串转换为新的字符数组。                               |
    | `String`        | `toLowerCase()`                                              | 使用默认语言环境的规则将此 `String`所有字符转换为小写。      |
    | `String`        | `toLowerCase(Locale locale)`                                 | 将此 `String`所有字符转换为小写，使用给定的 `Locale`的规则。 |
    | `String`        | `toString()`                                                 | 这个对象（已经是一个字符串！）                               |
    | `String`        | `toUpperCase()`                                              | 将此 `String`所有字符转换为大写，使用默认语言环境的规则。    |
    | `String`        | `toUpperCase(Locale locale)`                                 | 将此 `String`所有字符转换为大写，使用给定的 `Locale`的规则。 |
    | `String`        | `trim()`                                                     | 返回一个字符串，其值为此字符串，并删除任何前导和尾随空格。   |
    | `static String` | `valueOf(boolean b)`                                         | 返回 `boolean`参数的字符串表示形式。                         |
    | `static String` | `valueOf(char c)`                                            | 返回 `char`参数的字符串表示形式。                            |
    | `static String` | `valueOf(char[] data)`                                       | 返回 `char`数组参数的字符串表示形式。                        |
    | `static String` | `valueOf(char[] data,  int offset, int count)`               | 返回 `char`数组参数的特定子阵列的字符串表示形式。            |
    | `static String` | `valueOf(double d)`                                          | 返回 `double`参数的字符串表示形式。                          |
    | `static String` | `valueOf(float f)`                                           | 返回 `float`参数的字符串表示形式。                           |
    | `static String` | `valueOf(int i)`                                             | 返回 `int`参数的字符串表示形式。                             |
    | `static String` | `valueOf(long l)`                                            | 返回 `long`参数的字符串表示形式。                            |
    | `static String` | `valueOf(Object obj)`                                        | 返回 `Object`参数的字符串表示形式。                          |

## 3.字符串类型和基本类型转换

1. int类型转为String类型

   ```java
   String str = Integer.toString(int param)
   ```

2. String 转为int

   ```java
   int num = Integer.parseInt(String param);
   Integer num = Integer.valueOf(String param)
   ```

## 4.字符串比较

1. ==号的作用

   - 比较基本数据类型：比较的是具体的值
   - 比较引用数据类型：比较的是对象地址值

2. equals方法的作用

   - 方法介绍

     ```java
     public boolean equals(String s)	比较两个字符串内容是否相同，区分大小写
     ```

   - 示例

     ```java
     public class StringDemo02 {
         public static void main(String[] args) {
             //构造方法的方式得到对象
             char[] chs = {'a', 'b', 'c'};
             String s1 = new String(chs);
             String s2 = new String(chs);
     
             //直接赋值的方式得到对象
             String s3 = "abc";
             String s4 = "abc";
     
             //比较字符串对象地址是否相同
             System.out.println(s1 == s2);
             System.out.println(s1 == s3);
             System.out.println(s3 == s4);
             System.out.println("--------");
     
             //比较字符串内容是否相同
             System.out.println(s1.equals(s2));
             System.out.println(s1.equals(s3));
             System.out.println(s3.equals(s4));
         }
     }
     ```

字符串缓冲池：

1. 创建的程序在运行的时候会创建一个字符串缓冲池
2. str被放入缓冲池前会查询缓冲池，是否有相同内容的字符串，没有
3. str1被放入缓冲池前会查询缓冲池，是否有相同内容的字符串，如果有则会把str1指向str所指向的对象

## 5. StringBuilder类

概念：StringBuilder 是一个可变的字符串类，我们可以把它看成是一个容器，这里的可变指的是 StringBuilder 对象中的内容是可变的

1. stringBuilder和string类的区别

   - String类：内容是不可变的
   - StringBuilder类：内容是可变的

2. StringBuilder类的构造方法

   常用：

   | 方法名                             | 说明                                       |
   | ---------------------------------- | ------------------------------------------ |
   | public StringBuilder()             | 创建一个空白可变字符串对象，不含有任何内容 |
   | public StringBuilder(String   str) | 根据字符串的内容，来创建可变字符串对象     |

3. 示例：

   ```java
   public class StringBuilderDemo01 {
       public static void main(String[] args) {
           //public StringBuilder()：创建一个空白可变字符串对象，不含有任何内容
           StringBuilder sb = new StringBuilder();
           System.out.println("sb:" + sb);
           System.out.println("sb.length():" + sb.length());
   
           //public StringBuilder(String str)：根据字符串的内容，来创建可变字符串对象
           StringBuilder sb2 = new StringBuilder("hello");
           System.out.println("sb2:" + sb2);
           System.out.println("sb2.length():" + sb2.length());
       }
   }
   ```

4. StringBuilder常用方法

   | 方法名                                   | 说明                                                |
   | ---------------------------------------- | --------------------------------------------------- |
   | public   StringBuilder append (任意类型) | 添加数据，并返回对象本身                            |
   | public   StringBuilder reverse()         | 返回相反的字符序列                                  |
   | public   int   length()                  | 返回长度，实际存储值                                |
   | public   String toString()               | 通过toString()就可以实现把StringBuilder转换为String |

5. StringBuilder和String相互转换【应用】（StringBuffer同理）

   - StringBuilder转换为String

     ​        public String toString()：通过 toString() 就可以实现把 StringBuilder 转换为 String

   - String转换为StringBuilder

     ​        public StringBuilder(String s)：通过构造方法就可以实现把 String 转换为 StringBuilder

   - 示例代码：

     ```java
     public class StringBuilderDemo02 {
         public static void main(String[] args) {
             /*
             //StringBuilder 转换为 String
             StringBuilder sb = new StringBuilder();
             sb.append("hello");
     
             //String s = sb; //这个是错误的做法
     
             //public String toString()：通过 toString() 就可以实现把 StringBuilder 转换为 String
             String s = sb.toString();
             System.out.println(s);
             */
     
             //String 转换为 StringBuilder
             String s = "hello";
     
             //StringBuilder sb = s; //这个是错误的做法
     
             //public StringBuilder(String s)：通过构造方法就可以实现把 String 转换为 StringBuilder
             StringBuilder sb = new StringBuilder(s);
     
             System.out.println(sb);
         }
     }
     ```

     

## 6.StringBuffer类

概念：

- 线程安全，可变的字符序列。字符串缓冲区就像一个String ，但可以修改。在任何时间点，它包含一些特定的字符序列，但可以通过某些方法调用来更改序列的长度和内容。

- 字符串缓冲区可以安全地被多个线程使用。  这些方法在必要时进行同步，以便任何特定实例上的所有操作都按照与所涉及的各个线程所执行的方法调用顺序一致的顺序发生。 

1. StringBuffer的构造方法

   - - | Constructor                      | 描述                                                         |
       | -------------------------------- | ------------------------------------------------------------ |
       | `StringBuffer()`                 | 构造一个没有字符的字符串缓冲区，初始容量为16个字符。         |
       | `StringBuffer(int capacity)`     | 构造一个没有字符的字符串缓冲区和指定的初始容量。             |
       | `StringBuffer(CharSequence seq)` | 构造一个字符串缓冲区，其中包含与指定的 `CharSequence`相同的字符。 |
       | `StringBuffer(String str)`       | 构造一个初始化为指定字符串内容的字符串缓冲区                 |

2. StringBuffer常用方法

   - - | Modifier and Type | 方法                             | 描述                                   |
       | ----------------- | -------------------------------- | -------------------------------------- |
       | `StringBuffer`    | `append()`                       | 将任意参数的字符串**追加**到序列中。   |
       | `StringBuffer`    | `insert(int offset,  boolean b)` | 将任意参数的字符串**插入**到此序列中。 |

   

   

## 7.字符串拼接简介

- 当对字符串进行修改的时候，需要使用StringBuffer和StringBuilder类；
- 与String类不同，StringBuffer和StringBuilder类的对象能够被多次修改，并且不产生新的未使用对象

1. StringBuffer类实现字符串拼接

```java
StringBuffer sbf = new StringBuffer();//创建StringBuffer对象
sbf.append("1");
sbf.append("2");
sbf.append("3");
sbf.append("4");
System.out.println(sbf);//1234
```

2. StringBuilder类实现字符串拼接

```java
StringBuilder sbd = new StringBuilder();
sbd.append("1");
sbd.append("2");
sbd.append("3");
sbd.append("4");
System.out.println(sbd);//1234
```

StringBuffer与StringBuilder区别

- StringBuilder和StringBuffer之间的最大不同在于StringBuffer的方法是线程安全的（StringBuffer可以同步访问）；
- StringBuilder相较于StringBuffer有速度优势，所以多数情况下建议使用 StringBuilder类。但在应用程序要求线程安全的情况下，则必须使用StringBuffer类； 

3. 案例

   - 案例需求

     定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，

     ​	并在控制台输出结果。例如，数组为int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]

   - 代码实现

     ```java
     /*
         思路：
             1:定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
             2:定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回。
               返回值类型 String，参数列表 int[] arr
             3:在方法中用 StringBuilder 按照要求进行拼接，并把结果转成 String 返回
             4:调用方法，用一个变量接收结果
             5:输出结果
      */
     public class StringBuilderTest01 {
         public static void main(String[] args) {
             //定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
             int[] arr = {1, 2, 3};
     
             //调用方法，用一个变量接收结果
             String s = arrayToString(arr);
     
             //输出结果
             System.out.println("s:" + s);
     
         }
     
         //定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回
         /*
             两个明确：
                 返回值类型：String
                 参数：int[] arr
          */
         public static String arrayToString(int[] arr) {
             //在方法中用 StringBuilder 按照要求进行拼接，并把结果转成 String 返回
             StringBuilder sb = new StringBuilder();
     
             sb.append("[");
     
             for(int i=0; i<arr.length; i++) {
                 if(i == arr.length-1) {
                     sb.append(arr[i]);
                 } else {
                     sb.append(arr[i]).append(", ");
                 }
             }
     
             sb.append("]");
     
             String s = sb.toString();
     
             return  s;
         }
     }
     ```

## 8. 字符串反转升级版

1. 案例需求

   定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出结果

   ​	例如，键盘录入abc，输出结果 cba

2. 代码实现

   ```java
   /*
       思路：
           1:键盘录入一个字符串，用 Scanner 实现
           2:定义一个方法，实现字符串反转。返回值类型 String，参数 String s
           3:在方法中用StringBuilder实现字符串的反转，并把结果转成String返回
           4:调用方法，用一个变量接收结果
           5:输出结果
    */
   public class StringBuilderTest02 {
       public static void main(String[] args) {
           //键盘录入一个字符串，用 Scanner 实现
           Scanner sc = new Scanner(System.in);
   
           System.out.println("请输入一个字符串：");
           String line = sc.nextLine();
   
           //调用方法，用一个变量接收结果
           String s = myReverse(line);
   
           //输出结果
           System.out.println("s:" + s);
       }
   
       //定义一个方法，实现字符串反转。返回值类型 String，参数 String s
       /*
           两个明确：
               返回值类型：String
               参数：String s
        */
       public static String myReverse(String s) {
           //在方法中用StringBuilder实现字符串的反转，并把结果转成String返回
           //String --- StringBuilder --- reverse() --- String
   //        StringBuilder sb = new StringBuilder(s);
   //        sb.reverse();
   //        String ss = sb.toString();
   //        return ss;
   
          return new StringBuilder(s).reverse().toString();
       }
   }
   ```

   

## 9.用户登录案例

1. 案例需求

   已知用户名和密码，请用程序实现模拟用户登录。总共给三次机会，登录之后，给出相应的提示

2. 示例

   ```java
   /*
       思路：
           1:已知用户名和密码，定义两个字符串表示即可
           2:键盘录入要登录的用户名和密码，用 Scanner 实现
           3:拿键盘录入的用户名、密码和已知的用户名、密码进行比较，给出相应的提示。字符串的内容比较，用equals() 方法实现
           4:用循环实现多次机会，这里的次数明确，采用for循环实现，并在登录成功的时候，使用break结束循环
    */
   public class StringTest01 {
       public static void main(String[] args) {
           //已知用户名和密码，定义两个字符串表示即可
           String username = "itheima";
           String password = "czbk";
   
           //用循环实现多次机会，这里的次数明确，采用for循环实现，并在登录成功的时候，使用break结束循环
           for(int i=0; i<3; i++) {
   
               //键盘录入要登录的用户名和密码，用 Scanner 实现
               Scanner sc = new Scanner(System.in);
   
               System.out.println("请输入用户名：");
               String name = sc.nextLine();
   
               System.out.println("请输入密码：");
               String pwd = sc.nextLine();
   
               //拿键盘录入的用户名、密码和已知的用户名、密码进行比较，给出相应的提示。字符串的内容比较，用equals() 方法实现
               if (name.equals(username) && pwd.equals(password)) {
                   System.out.println("登录成功");
                   break;
               } else {
                   if(2-i == 0) {
                       System.out.println("你的账户被锁定，请与管理员联系");
                   } else {
                       //2,1,0
                       //i,0,1,2
                       System.out.println("登录失败，你还有" + (2 - i) + "次机会");
                   }
               }
           }
       }
   }
   ```

## 10用户登录案例

1. 案例需求

   键盘录入一个字符串，使用程序实现在控制台遍历该字符串

2. 代码实现

   ```java
   /*
       思路：
           1:键盘录入一个字符串，用 Scanner 实现
           2:遍历字符串，首先要能够获取到字符串中的每一个字符
               public char charAt(int index)：返回指定索引处的char值，字符串的索引也是从0开始的
           3:遍历字符串，其次要能够获取到字符串的长度
               public int length()：返回此字符串的长度
               数组的长度：数组名.length
               字符串的长度：字符串对象.length()
           4:遍历字符串的通用格式
    */
   public class StringTest02 {
       public static void main(String[] args) {
           //键盘录入一个字符串，用 Scanner 实现
           Scanner sc = new Scanner(System.in);
   
           System.out.println("请输入一个字符串：");
           String line = sc.nextLine();
   
           for(int i=0; i<line.length(); i++) {
               System.out.println(line.charAt(i));
           }
       }
   }
   ```

   

## 11.统计字符次数案例

1. 案例需求

   键盘录入一个字符串，统计该字符串中大写字母字符，小写字母字符和数字字符出现的次数

2. 代码实现

   ```java
   /*
     思路：
           1:键盘录入一个字符串，用 Scanner 实现
           2:要统计三种类型的字符个数，需定义三个统计变量，初始值都为0
           3:遍历字符串，得到每一个字符
           4:判断该字符属于哪种类型，然后对应类型的统计变量+1
               假如ch是一个字符，我要判断它属于大写字母，小写字母，还是数字，直接判断该字符是否在对应的范围即可
               大写字母：ch>='A' && ch<='Z'
               小写字母： ch>='a' && ch<='z'
               数字： ch>='0' && ch<='9'
           5:输出三种类型的字符个数
    */
   public class StringTest03 {
       public static void main(String[] args) {
           //键盘录入一个字符串，用 Scanner 实现
           Scanner sc = new Scanner(System.in);
   
           System.out.println("请输入一个字符串：");
           String line = sc.nextLine();
   
           //要统计三种类型的字符个数，需定义三个统计变量，初始值都为0
           int bigCount = 0;
           int smallCount = 0;
           int numberCount = 0;
   
           //遍历字符串，得到每一个字符
           for(int i=0; i<line.length(); i++) {
               char ch = line.charAt(i);
   
               //判断该字符属于哪种类型，然后对应类型的统计变量+1
               if(ch>='A' && ch<='Z') {
                   bigCount++;
               } else if(ch>='a' && ch<='z') {
                   smallCount++;
               } else if(ch>='0' && ch<='9') {
                   numberCount++;
               }
           }
   
           //输出三种类型的字符个数
           System.out.println("大写字母：" + bigCount + "个");
           System.out.println("小写字母：" + smallCount + "个");
           System.out.println("数字：" + numberCount + "个");
       }
   }
   ```

## 12.字符串拼接案例

1. 案例需求

   定义一个方法，把 int 数组中的数据按照指定的格式拼接成一个字符串返回，调用该方法，

   ​	并在控制台输出结果。例如，数组为 int[] arr = {1,2,3}; ，执行方法后的输出结果为：[1, 2, 3]

2. 代码实现

   ```java
   /*
       思路：
           1:定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
           2:定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回。
             返回值类型 String，参数列表 int[] arr
           3:在方法中遍历数组，按照要求进行拼接
           4:调用方法，用一个变量接收结果
           5:输出结果
    */
   public class StringTest04 {
       public static void main(String[] args) {
           //定义一个 int 类型的数组，用静态初始化完成数组元素的初始化
           int[] arr = {1, 2, 3};
   
           //调用方法，用一个变量接收结果
           String s = arrayToString(arr);
   
           //输出结果
           System.out.println("s:" + s);
       }
   
       //定义一个方法，用于把 int 数组中的数据按照指定格式拼接成一个字符串返回
       /*
           两个明确：
               返回值类型：String
               参数：int[] arr
        */
       public static String arrayToString(int[] arr) {
           //在方法中遍历数组，按照要求进行拼接
           String s = "";
   
           s += "[";
   
           for(int i=0; i<arr.length; i++) {
               if(i==arr.length-1) {
                   s += arr[i];
               } else {
                   s += arr[i];
                   s += ", ";
               }
           }
   
           s += "]";
   
           return s;
       }
   }
   ```

## 13.字符串反转案例(简单版)

1. 案例需求

   定义一个方法，实现字符串反转。键盘录入一个字符串，调用该方法后，在控制台输出结果

   ​	例如，键盘录入 abc，输出结果 cba

2. 代码实现

   ```java
   /*
       思路：
           1:键盘录入一个字符串，用 Scanner 实现
           2:定义一个方法，实现字符串反转。返回值类型 String，参数 String s
           3:在方法中把字符串倒着遍历，然后把每一个得到的字符拼接成一个字符串并返回
           4:调用方法，用一个变量接收结果
           5:输出结果
    */
   public class StringTest05 {
       public static void main(String[] args) {
           //键盘录入一个字符串，用 Scanner 实现
           Scanner sc = new Scanner(System.in);
   
           System.out.println("请输入一个字符串：");
           String line = sc.nextLine();
   
           //调用方法，用一个变量接收结果
           String s = reverse(line);
   
           //输出结果
           System.out.println("s:" + s);
       }
   
       //定义一个方法，实现字符串反转
       /*
           两个明确：
               返回值类型：String
               参数：String s
        */
       public static String reverse(String s) {
           //在方法中把字符串倒着遍历，然后把每一个得到的字符拼接成一个字符串并返回
           String ss = "";
   
           for(int i=s.length()-1; i>=0; i--) {
               ss += s.charAt(i);
           }
   
           return ss;
       }
   }
   ```

   