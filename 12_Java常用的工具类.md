## 1.日期类

### 1.Date类及使用

Date类构造方法

| 方法名                 | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| public Date()          | 分配一个 Date对象，并初始化，以便它代表它被分配的时间，精确到毫秒 |
| public Date(long date) | 分配一个 Date对象，并将其初始化为表示从标准基准时间起指定的毫秒数 |

```java
Date date = new date();//默认格式：Fri Mar 15 16:13:21 CST 2019
```

示例：

```java
public class DateDemo01 {
    public static void main(String[] args) {
        //public Date()：分配一个 Date对象，并初始化，以便它代表它被分配的时间，精确到毫秒
        Date d1 = new Date();
        System.out.println(d1);

        //public Date(long date)：分配一个 Date对象，并将其初始化为表示从标准基准时间起指定的毫秒数
        long date = 1000*60*60;
        Date d2 = new Date(date);
        System.out.println(d2);
    }
}
```



### 2. 日期格式化及SimpleDateFormat的使用

- SimpleDateFormat类概述：

  - SimpleDateFormat是一个具体的类，用于以区域设置敏感的方式格式化和解析日期。

- SimpleDateFormat类构造方法

  | 方法名                                  | 说明                                                   |
  | --------------------------------------- | ------------------------------------------------------ |
  | public   SimpleDateFormat()             | 构造一个SimpleDateFormat，使用默认模式和日期格式       |
  | public SimpleDateFormat(String pattern) | 构造一个SimpleDateFormat使用给定的模式和默认的日期格式 |

- SimpleDateFormat类的常用方法

  - 格式化(从Date到String)
    - public final String format(Date date)：将日期格式化成日期/时间字符串
  - 解析(从String到Date)
    - public Date parse(String source)：从给定字符串的开始解析文本以生成日期

- 示例

  ```java
  public class SimpleDateFormatDemo {
      public static void main(String[] args) throws ParseException {
          //格式化：从 Date 到 String
          Date d = new Date();
  //        SimpleDateFormat sdf = new SimpleDateFormat();
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
          String s = sdf.format(d);
          System.out.println(s);
          System.out.println("--------");
  
          //从 String 到 Date
          String ss = "2048-08-09 11:11:11";
          //ParseException
          SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
          Date dd = sdf2.parse(ss);
          System.out.println(dd);
      }
  }
  ```

- 日期格式化：

```java
//格式化
SimpleDateFormat format = new SimpleDateFormat("yyy-MM-dd hh:mm:ss");//具体参数自行百度

//输出结果
System.out.println(format.format(date));
```



### 3.Calendar类及使用

- Calendar类概述

  ​	Calendar 为特定瞬间与一组日历字段之间的转换提供了一些方法，并为操作日历字段提供了一些方法

  ​	Calendar 提供了一个类方法 getInstance 用于获取这种类型的一般有用的对象。

  ​	该方法返回一个Calendar 对象。

  ​	其日历字段已使用当前日期和时间初始化：Calendar rightNow = Calendar.getInstance();

- Calendar类常用方法

  | 方法名                                             | 说明                                                   |
  | -------------------------------------------------- | ------------------------------------------------------ |
  | public int   get(int field)                        | 返回给定日历字段的值                                   |
  | public abstract void add(int   field, int amount)  | 根据日历的规则，将指定的时间量添加或减去给定的日历字段 |
  | public final void set(int year,int month,int date) | 设置当前日历的年月日                                   |

- 示例：

  ```java
  public class CalendarDemo {
      public static void main(String[] args) {
          //获取日历类对象
          Calendar c = Calendar.getInstance();
  
          //public int get(int field):返回给定日历字段的值
          int year = c.get(Calendar.YEAR);
          int month = c.get(Calendar.MONTH) + 1;
          int date = c.get(Calendar.DATE);
          System.out.println(year + "年" + month + "月" + date + "日");
  
          //public abstract void add(int field, int amount):根据日历的规则，将指定的时间量添加或减去给定的日历字段
          //需求1:3年前的今天
  //        c.add(Calendar.YEAR,-3);
  //        year = c.get(Calendar.YEAR);
  //        month = c.get(Calendar.MONTH) + 1;
  //        date = c.get(Calendar.DATE);
  //        System.out.println(year + "年" + month + "月" + date + "日");
  
          //需求2:10年后的10天前
  //        c.add(Calendar.YEAR,10);
  //        c.add(Calendar.DATE,-10);
  //        year = c.get(Calendar.YEAR);
  //        month = c.get(Calendar.MONTH) + 1;
  //        date = c.get(Calendar.DATE);
  //        System.out.println(year + "年" + month + "月" + date + "日");
  
          //public final void set(int year,int month,int date):设置当前日历的年月日
          c.set(2050,10,10);
          year = c.get(Calendar.YEAR);
          month = c.get(Calendar.MONTH) + 1;
          date = c.get(Calendar.DATE);
          System.out.println(year + "年" + month + "月" + date + "日");
  
      }
  }
  ```

- 案例：二月天

  1. 案例需求

     ​	获取任意一年的二月有多少天

  2. 代码实现

     ```java
     public class CalendarTest {
         public static void main(String[] args) {
             //键盘录入任意的年份
             Scanner sc = new Scanner(System.in);
             System.out.println("请输入年：");
             int year = sc.nextInt();
     
             //设置日历对象的年、月、日
             Calendar c = Calendar.getInstance();
             c.set(year, 2, 1);
     
             //3月1日往前推一天，就是2月的最后一天
             c.add(Calendar.DATE, -1);
     
             //获取这一天输出即可
             int date = c.get(Calendar.DATE);
             System.out.println(year + "年的2月份有" + date + "天");
         }
     }
     ```

     



### 4.Date类常用方法（应用）

| 方法名                         | 说明                                                  |
| ------------------------------ | ----------------------------------------------------- |
| public long getTime()          | 获取的是日期对象从1970年1月1日 00:00:00到现在的毫秒值 |
| public void setTime(long time) | 设置时间，给的是毫秒值                                |

示例代码：

```java
public class DateDemo02 {
    public static void main(String[] args) {
        //创建日期对象
        Date d = new Date();

        //public long getTime():获取的是日期对象从1970年1月1日 00:00:00到现在的毫秒值
//        System.out.println(d.getTime());
//        System.out.println(d.getTime() * 1.0 / 1000 / 60 / 60 / 24 / 365 + "年");

        //public void setTime(long time):设置时间，给的是毫秒值
//        long time = 1000*60*60;
        long time = System.currentTimeMillis();
        d.setTime(time);

        System.out.println(d);
    }
}
```

### 5. 日期工具类案例（应用）

1. 案例需求

   定义一个日期工具类(DateUtils)，包含两个方法：把日期转换为指定格式的字符串；把字符串解析为指定格式的日期，然后定义一个测试类(DateDemo)，测试日期工具类的方法

2. 代码实现

   1. 工具类

      ```java
      public class DateUtils {
          private DateUtils() {}
      
          /*
              把日期转为指定格式的字符串
              返回值类型：String
              参数：Date date, String format
           */
          public static String dateToString(Date date, String format) {
              SimpleDateFormat sdf = new SimpleDateFormat(format);
              String s = sdf.format(date);
              return s;
          }
      
      
          /*
              把字符串解析为指定格式的日期
              返回值类型：Date
              参数：String s, String format
           */
          public static Date stringToDate(String s, String format) throws ParseException {
              SimpleDateFormat sdf = new SimpleDateFormat(format);
              Date d = sdf.parse(s);
              return d;
          }
      
      }
      ```

   2. 测试类

      ```java
      public class DateDemo {
          public static void main(String[] args) throws ParseException {
              //创建日期对象
              Date d = new Date();
      
              String s1 = DateUtils.dateToString(d, "yyyy年MM月dd日 HH:mm:ss");
              System.out.println(s1);
      
              String s2 = DateUtils.dateToString(d, "yyyy年MM月dd日");
              System.out.println(s2);
      
              String s3 = DateUtils.dateToString(d, "HH:mm:ss");
              System.out.println(s3);
              System.out.println("--------");
      
              String s = "2048-08-09 12:12:12";
              Date dd = DateUtils.stringToDate(s, "yyyy-MM-dd HH:mm:ss");
              System.out.println(dd);
          }
      }
      ```

      

## 2.Math类的使用

- Math类概述

  - Math 包含执行基本数字运算的方法

- Math中方法的调用方式

  - Math类中无构造方法，但内部的方法都是静态的，则可以通过   **类名.进行调用**

- Math类的常用方法

  | 方法名    方法名                               | 说明                                           |
  | ---------------------------------------------- | ---------------------------------------------- |
  | public static int   abs(int a)                 | 返回参数的绝对值                               |
  | public static double ceil(double a)            | 返回大于或等于参数的最小double值，等于一个整数 |
  | public static double floor(double a)           | 返回小于或等于参数的最大double值，等于一个整数 |
  | public   static int round(float a)             | 按照四舍五入返回最接近参数的int                |
  | public static int   max(int a,int b)           | 返回两个int值中的较大值                        |
  | public   static int min(int a,int b)           | 返回两个int值中的较小值                        |
  | public   static double pow (double a,double b) | 返回a的b次幂的值                               |
  | public   static double random()                | 返回值为double的正值，[0.0,1.0)                |



## 3.System

System类的常用方法 

| 方法名                                   | 说明                                             |
| ---------------------------------------- | ------------------------------------------------ |
| public   static void exit(int status)    | 终止当前运行的   Java   虚拟机，非零表示异常终止 |
| public   static long currentTimeMillis() | 返回当前时间(以毫秒为单位)                       |

示例代码

- 需求：在控制台输出1-10000，计算这段代码执行了多少毫秒 

  ```java
  public class SystemDemo {
      public static void main(String[] args) {
          // 获取开始的时间节点
          long start = System.currentTimeMillis();
          for (int i = 1; i <= 10000; i++) {
              System.out.println(i);
          }
          // 获取代码运行结束后的时间节点
          long end = System.currentTimeMillis();
          System.out.println("共耗时：" + (end - start) + "毫秒");
      }
  }
  ```

  







































