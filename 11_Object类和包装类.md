# 11_Object类和包装类

## 1. Object 类

1. Object类介绍

   Object 是类层次结构的根，每个类都可以将 Object 作为超类。所有类都直接或者间接的继承自该类，换句话说，该类所具备的方法，所有类都会有一份

2. Object类常见方法
   1. public native int hashCode();

      作用：返回对象的哈希码

   2. public boolean equals(Object obj);

      作用：用于比较

3. object类的toString方法

   toString方法的作用：以良好的格式，更方便的展示对象中的属性值

   案例：

   ```java
   class Student extends Object {
       private String name;
       private int age;
   
       public Student() {
       }
   
       public Student(String name, int age) {
           this.name = name;
           this.age = age;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       @Override
       public String toString() {
           return "Student{" +
                   "name='" + name + '\'' +
                   ", age=" + age +
                   '}';
       }
   }
   public class ObjectDemo {
       public static void main(String[] args) {
           Student s = new Student();
           s.setName("林青霞");
           s.setAge(30);
           System.out.println(s); 
           System.out.println(s.toString()); 
       }
   }
   ```

   运行结果

   ```java
   Student{name='林青霞', age=30}
   Student{name='林青霞', age=30}
   ```

   

## 2.==号与equals方法比较及equals方法重写

1.  == 号

   如果作用于基本数据类型变量，直接比较值

   如果作用域引用类型的变量，比较的是所指向的内存地址

2. equals方法

   如果**没有**对equals方法进行重写，比较的是所指向的对象的地址

   如果**已经**对equals方法进行了重写，就按照重写的逻辑进行处理

   注意：equals方法**不能用于**比较基本数据类型变量
   
   - equals方法的作用：
     - **用于对象之间的比较，返回true和false的结果**
     - 举例：s1.equals(s2);    s1和s2是两个对象
   
   - 重写equals方法的场景
     - 不希望比较对象的地址值，想要结合对象属性进行比较的时候。
   
   - 重写equals方法的方式
     - alt + insert  选择equals() and hashCode()，IntelliJ Default，一路next，finish即可
     - 在类的空白区域，右键 -> Generate -> 选择equals() and hashCode()，后面的同上。
   
   equals案例：
   
   ```java
   class Student {
       private String name;
       private int age;
   
       public Student() {
       }
   
       public Student(String name, int age) {
           this.name = name;
           this.age = age;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       @Override
       public boolean equals(Object o) {
           //this -- s1
           //o -- s2
           if (this == o) return true;
           if (o == null || getClass() != o.getClass()) return false;
   
           Student student = (Student) o; //student -- s2
   
           if (age != student.age) return false;
           return name != null ? name.equals(student.name) : student.name == null;
       }
   }
   public class ObjectDemo {
       public static void main(String[] args) {
           Student s1 = new Student();
           s1.setName("林青霞");
           s1.setAge(30);
   
           Student s2 = new Student();
           s2.setName("林青霞");
           s2.setAge(30);
   
           //需求：比较两个对象的内容是否相同
           System.out.println(s1.equals(s2));
       }
   }
   ```
   
   

## 3. hashCode方法和重写

1. hashCode概要

   如果两个对象相同，equals一定要返回true，且两个对象的HashCode一定相同；

   如果两个对象的HashCode相同，两个对象不一定相同，即equals()的值不一定为true，只能表明这两个对象在一个散列存储结构中

2. HashCode的作用

   - 提高了读取对象的效率

     ​	从Object角度看，JVM【Java虚拟机】new一个Object，它都会将这个Object丢到一个Hash表中去，下次做Object的比较或者取这个对象的时候【读取操作】，它会根据对象的HashCode再从Hash表中取这个对象。这样做的目的是提高读取对象的效率。若HashCode相同再去调用equals。

   - hashSet等存储数据的依据

     ​	hashCode是一个标识，到散列表中去找该对象的一个标识，如果相同的hashCode的类放于相同的散列表中，然后在通过equals进一步选出散列表中对应的对象。因此相同标识下的类不能重复

## 4. Object类的其他方法应用

- - | `protected Object` | `clone()`            | 创建并返回此对象的副本。                                     |
    | ------------------ | -------------------- | ------------------------------------------------------------ |
    | `boolean`          | `equals(Object obj)` | 指示一些其他对象是否等于此。                                 |
    | `Class<?>`         | `getClass()`         | 返回此 `Object`的运行时类。                                  |
    | `int`              | `hashCode()`         | 返回对象的哈希码值。                                         |
    | `void`             | `notify()`           | 唤醒正在等待对象监视器的单个线程。                           |
    | `void`             | `notifyAll()`        | 唤醒正在等待对象监视器的所有线程。                           |
    | `String`           | `toString()`         | 返回对象的字符串表示形式。                                   |
    | `void`             | `wait()`             | 导致当前线程等待，直到另一个线程调用该对象的 [`notify()`](../../java/lang/Object.html#notify--)方法或 [`notifyAll()`](../../java/lang/Object.html#notifyAll--)方法。 |

## 5.包装类及其使用

### 1. 什么是包装类

为了支持java语言面向对象的特性，基本数据类型中的每一个在java.lang包中都有一个对应的包装类，即将每个基本类型都包装成一个类

| 基本类型 | 包装类                  |
| -------- | ----------------------- |
| byte     | java.lang.Byte          |
| short    | java.lang.Short         |
| int      | **java.lang.Integer**   |
| long     | java.lang.Long          |
| float    | java.lang.Float         |
| double   | java.lang.Double        |
| char     | **java.lang.Character** |
| boolean  | java.lang.Boolean       |

### 2.包装类和基本类型的转换

1. 基础类型转换为包装类【又称为**装箱操作**，装箱又分为手动装箱和自动装箱】

   ```java
   int a = 1;//定义int类型变量
   Integer b = new Integer(a);//手动装箱 ---- 基本数据类型转为包装类
   Integer c = Integer.valueOf(a);//手动装箱 ---- 基本数据类型转为包装类
   Integer d = a;//自动装箱 ---- 基本数据类型转为包装类
   ```

2. 包装类转换为基础类型【又称为**拆箱操作**，拆箱又分为手动拆箱和自动拆箱】

   ```java
   Integer x = new Integer(100);//定义Integer类型的包装对象
   int y = x.intValue();//手动拆箱 ---- 包装类转为基本数据类型
   Integer z = x;//自动拆箱 ---- 包装类转为基本数据类型
   ```
   
   
   
   自动装箱
   
   ​	把基本数据类型转换为对应的包装类类型
   
   自动拆箱
   
   ​	把包装类类型转换为对应的基本数据类型

### 3. 包装类的作用

1. 包含了每种基本数据类型的相关属性，如最大值，最小值等
2. 集合不允许存放基本数据类型，只允许存放对象类，故常用包装类
3. 作为基本数据类型对应的类型，提供了一系列使用的对象操作，如：类型转换、进制转换等

### 4.Integer类

- Integer类概述

  ​	包装一个对象中的原始类型 int 的值

- Integer类构造方法

  | 方法名                                  | 说明                                     |
  | --------------------------------------- | ---------------------------------------- |
  | public Integer(int   value)             | 根据 int 值创建 Integer 对象(过时)       |
  | public Integer(String s)                | 根据 String 值创建 Integer 对象(过时)    |
  | public static Integer valueOf(int i)    | 返回表示指定的 int 值的 Integer   实例   |
  | public static Integer valueOf(String s) | 返回一个保存指定值的 Integer 对象 String |

- 示例

  ```java
  public class IntegerDemo {
      public static void main(String[] args) {
          //public Integer(int value)：根据 int 值创建 Integer 对象(过时)
          Integer i1 = new Integer(100);
          System.out.println(i1);
  
          //public Integer(String s)：根据 String 值创建 Integer 对象(过时)
          Integer i2 = new Integer("100");
  //        Integer i2 = new Integer("abc"); //NumberFormatException
          System.out.println(i2);
          System.out.println("--------");
  
          //public static Integer valueOf(int i)：返回表示指定的 int 值的 Integer 实例
          Integer i3 = Integer.valueOf(100);
          System.out.println(i3);
  
          //public static Integer valueOf(String s)：返回一个保存指定值的Integer对象 String
          Integer i4 = Integer.valueOf("100");
          System.out.println(i4);
      }
  }
  ```

  ### 5.int和String类型的相互转换

  int转换为String

  - 转换方式

    - 方式一：直接在数字后加一个空字符串
    - 方式二：通过String类静态方法valueOf()

  - 示例代码

    ```java
    public class IntegerDemo {
        public static void main(String[] args) {
            //int --- String
            int number = 100;
            //方式1
            String s1 = number + "";
            System.out.println(s1);
            //方式2
            //public static String valueOf(int i)
            String s2 = String.valueOf(number);
            System.out.println(s2);
            System.out.println("--------");
        }
    }
    ```

  String转换为int

  - 转换方式

    - 方式一：先将字符串数字转成Integer，再调用valueOf()方法
    - 方式二：通过Integer静态方法parseInt()进行转换

  - 示例代码

    ```java
    public class IntegerDemo {
        public static void main(String[] args) {
            //String --- int
            String s = "100";
            //方式1：String --- Integer --- int
            Integer i = Integer.valueOf(s);
            //public int intValue()
            int x = i.intValue();
            System.out.println(x);
            //方式2
            //public static int parseInt(String s)
            int y = Integer.parseInt(s);
            System.out.println(y);
        }
    }
    ```

    