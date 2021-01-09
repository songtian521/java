## 1. java类和对象的概念

1. 类
   - 类的理解
     - 类是对现实生活中一类具有共同属性和行为的事物的抽象
     - 类是对象的数据类型，类是具有相同属性和行为的一组对象的集合
     - 简单理解：类就是对现实事物的一种描述
   - 类的组成
     - 属性：指事物的特征，例如：手机事物（品牌，价格，尺寸）
     - 行为：指事物能执行的操作，例如：手机事物（打电话，发短信）

2. 类和对象的关系
   - 类：类是对现实生活中一类具有共同属性和行为的事物的抽象
   - 对象：是能够看得到摸的着的真实存在的实体
   - 简单理解：**类是对事物的一种描述，对象则为具体存在的事物**

## 2.类的定义

1. class关键字

   用来定义类的关键字

2. 类的定义和组成
   - 类的定义

     [访问权限修饰符] [修饰符] class 类名 {类体}

     ```java
     public class Test{
         //代码块
     }
     
     public class Phone {
         //成员变量
         String brand;
         int price;
     
         //成员方法
         public void call() {
             System.out.println("打电话");
         }
     
         public void sendMessage() {
             System.out.println("发短信");
         }
     }
     
     ```

3. 类的组成

   概念：类是由属性和行为两部分组成

   - 属性：在类中通过成员变量来体现（类中方法外的变量）
   - 行为：在类中通过成员方法来体现（和前面的方法相比去掉static关键字即可）

## 3.对象的创建（使用）

在java中对象是根据类创建的,使用个关键字new来创建一个新对象.创建对象需要以下三步:

1. 声明:声明一个对象,包括对象名称和对象类型

2. 实例化:使用关键字new来创建一个对象

3. 初始化: 使用new 创建对象时，会调用构造方法初始化对象

   **创建格式**：类名称 对象名 = new 类名称（）；

   ```java
   Student stu = new Student();
   ```

   含义：根据student类创建一个名为stu的对象
   
4. 调用成员的格式：

   - 对象名.成员变量
   - 对象名.成员方法();

案例：

```java
public class PhoneDemo {
    public static void main(String[] args) {
        //创建对象
        Phone p = new Phone();

        //使用成员变量
        System.out.println(p.brand);
        System.out.println(p.price);

        p.brand = "小米";
        p.price = 2999;

        System.out.println(p.brand);
        System.out.println(p.price);

        //使用成员方法
        p.call();
        p.sendMessage();
    }
}
```



## 4.成员变量和成员方法

- 成员变量: 可以简单理解为类中的方法以外定义的变量
- 成员方法: 类中的所有方法**[构造方法除外]**称为成员方法,成员方法包括实例方法和静态方法

```java
public class test{//一个类
	public int y;//成员变量/实例变量
    public static int z;//类变量
	
    //静态初始器: 由static和{}组成 只在类装载的时候执行一次(第一次使用类的时候),一般用于初始化静态变量
    //注意:成员变量无法在静态初始器进行初始化
    static{
        z = 2;
    }
    
    //实例方法
	public void a(){ //非静态方法可以访问非静态数据成员和静态数据成员
        System.out.println("实例方法 成员变量 : " + y);//0
		System.out.println("实例方法 静态变量 : " + z);//2
        //代码块
	}
    
    //静态方法   
	public static void b(){//静态方法只能访问静态数据成员
        System.out.println("静态方法 静态变量 : " + z);//2
        //代码块
	}
    public static void main(String[] args) {
		test test = new test();
		test.a();
		test.b();
	}
    
}
```

**注意：**成员变量和局部变量对比

- 类中位置不同：成员变量（类中方法外）局部变量（方法内部或方法声明上）
- 内存中位置不同：成员变量（堆内存）局部变量（栈内存）
- 生命周期不同：成员变量（随着对象的存在而存在，随着对象的消失而消失）局部变量（随着方法的调用而存在，醉着方法的调用完毕而消失）
- 初始化值不同：成员变量（有默认初始化值）局部变量（没有默认初始化值，必须先定义，赋值才能使用）

## 5.构造方法及其重载

### 1.构造方法定义

java构造函数，也叫构造方法，是java中一种特殊的函数。函数名与类名相同，无返回值。

注意：

- 构造方法的创建

  如果没有定义构造方法，系统将给出一个默认的无参数构造方法
  如果定义了构造方法，系统将不再提供默认的构造方法

- 构造方法的重载

  如果自定义了带参构造方法，还要使用无参数构造方法，就必须再写一个无参数构造方法

- 推荐的使用方式

  无论是否使用，都手工书写无参数构造方法

- 重要功能！

  可以使用带参构造，为成员变量进行初始化

示例：

```java
/*
    学生类
 */
class Student {
    private String name;
    private int age;

    public Student() {}

    public Student(String name) {
        this.name = name;
    }

    public Student(int age) {
        this.age = age;
    }

    public Student(String name,int age) {
        this.name = name;
        this.age = age;
    }

    public void show() {
        System.out.println(name + "," + age);
    }
}
/*
    测试类
 */
public class StudentDemo {
    public static void main(String[] args) {
        //创建对象
        Student s1 = new Student();
        s1.show();

        //public Student(String name)
        Student s2 = new Student("林青霞");
        s2.show();

        //public Student(int age)
        Student s3 = new Student(30);
        s3.show();

        //public Student(String name,int age)
        Student s4 = new Student("林青霞",30);
        s4.show();
    }
}
```



### 2.构造函数的具体定义方法

- 作用：创建对象   Student stu = **new Student();**

- 格式：

  public class 类名{

  ​        修饰符 类名( 参数 ) {

  ​        }

  }

- 功能：主要是完成对象数据的初始化

```java
public class abc {
	public abc() {//无参构造函数
		System.out.println("1");
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		abc abc = new abc();
	}

}
```

### 3.构造方法的作用

- 创建对象

  任何类想要创建实例对象就必须调用构造函数

- 对象初始化

  构造函数可以对对象进行初始化，并且是给与之格式（参数列表）相符合的对象初始化，是具有一定针对性的初始函数

### 4.构造方法与普通方法的区别

- 格式不同

  构造函数不存在返回类型，函数名与所在类的类名一致；

  普通函数有返回类型，函数名可以根据需求进行命名；

- 调用时期不同

  构造函数在类的对象创建时就会运行

  普通函数在对象调用时才会执行

- 调用次数不同

  一个对象创建后，其构造函数只执行一次，就是创建时执行

  一个对象创建后，其普通函数可以执行多次，取决于对象的调用次数

### 5.构造方法的重载

和方法的重载相同

函数名相同，参数列表不同

## 6. this关键字

1. this的作用
   - this可以看做是一个变量，它的值是当前对象的引用
   - 方法中使用this关键字代表使用该方法对象的引用
   - this可以处理方法中成员变量和方法参数重名问题
   
2. this总结
   - this 一般出现在方法里面，当这个方法还没有调用的时候，thsi具体指的是哪个对象并不知道
   - 如果new一个对象出来，那么this指的就是当前的这个对象
   - 那个对象调用方法，this指的就是调用这个方法的对象（那个对象调用这个方法，this指的就是该对象）
   - 如果在new一个对象，这个对象也有自己的this，新对象的this就就当然指它自己了
   
3. this内存原理

   概念：this代表当前调用方法的引用，哪个对象调用的方法，this就代表哪一个对象

   示例：

   ```java
   public class StudentDemo {
       public static void main(String[] args) {
           Student s1 = new Student();
           s1.setName("林青霞");
           Student s2 = new Student();
           s2.setName("张曼玉");
       }
   }
   ```

   

示例：

```java
public class Student {
    private String name;
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    public void show() {
        System.out.println(name + "," + age);
    }
}
```
