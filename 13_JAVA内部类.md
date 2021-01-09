## 1.内部类简介

### 1.什么是内部类

在一个类中定义另外的一个类

```java
public class DefineInnerClass{

	//内部类
	public class InnerClass{
        
	}
}
```

### 2.内部类的概念

1. 内部类可以对外围类的属性访问

   解析：当外部类创建了一个内部类的对象的时候，内部类会捕获一个指向外部类对象的引用，然后访问外部类的成员时，就用那个引用来选择外围类的成员，编译器会自动帮我们处理

2. 引用内部类的需要指明这个对象的类型

3. 创建内部类对象必须要利用外部类的对象

4. 内部类是编译期的概念，编译完成后就是两个独立的类

5. 推荐使用get方法获取内部类

   ```java
   //定义一个外围类
   class DefineInnerClass{
   	//定义外围类的属性
       private String name;
       private int age;
       //实现外围类的get，set方法
       public String getName(){
           return name;
       }
       public void setName(String name){
           this.name = name;
       }
        public int getAge(){
           return age;
       }
       public void setName(int age){
           this.age = age;
       }
       //内部类
       class InnerClass{
       	//内部类的无参构造
           public InnerClass(){
               name = "宋天";
               age = 21;
           }
           public void display(){
               System.out.println("name : " + getName() + ", age : " + getAge());
           }
       }
   }	
   
   //定义主函数
   public class TestMain{
       public static void main(String[] args){
           //实例化DefineInnerClass
           DefineInnerClass dc = new DefineInnerClass();
           //创建内部类
           DefineInnerClass.InnerClass innerClass = dc.new InnerClass();
           innerClass.display();
       } 
   }
   ```

### 3. 内部类实现对外隐藏

普通的类不能使用private，protected访问修饰符来修饰，而内部类可以，当我们使用private修饰内部类的时候就可以实现对外隐藏，比如：实现接口，向上转型，对于用户来说，就完全实现了接口的实现，只需要调用一个方法就能帮助实现想要实现的复杂功能

```java
//定义接口
public interface InterfaceHide{
    void increment();
}

//定义实现类
public class HideRealize{
	//定义内部类 实现接口
    public class InnerClass implements InterfaceHide{
    	@Override
    	public void increment(){
        	System.out.println("");
 		}
 	}   	
 	public InterfaceHide getInt(){
        return new InnerClass();
 	}
}

//主函数
public class HideMain{
    public static void main(String[] args){
        //创建外部类
        HideRealize hide = new HideRealize();
        //获取内部类
        InterfaceHide innterfaceHide = hide.getInet();
        //调用实现接口方法
        innterfaceHide.increment();
    }
}

```

### 4.内部类多继承

在java中只能继承一个类，如果没有内部类，只能够通过实现接口来实现。但是实现接口需要覆盖接口中所有的方法。外部类通过内部类拥有了其他类的继承关系，而且不必关心被继承类的具体方法实现

```java
//定义父类1
public class Chip{
    public String descChip(){
        return "高通骁龙855";
    }
}
//定义父类2
public class Meonry{
    public String Manufacturer(){
        return "三星";
    }
}

//实现继承
public class Phone{
    //内部类1 继承chilp
    private class partOne extends Chilp{
        @Override
        public String descChip(){
            return new super.descChip();
        }
    }
    //内部类2 继承Meonry
    private class partTwo extends Meonry{
        @Override
        public String Manufacturer(){
            return new super.Manufacturer();
        }
    }
    public String getChip(){
        return new partOne().descChip();
    }
    public String getMeonry(){
        return new parTwo().Manufacturer();
    }
}

//主函数执行
public class MainPhone{
    public static void main(String[] args){
        //创建外围类
        Phone p1 = new Phone();
        System.out.println("手机芯片是：" + p1.getChip());//高通骁龙855
        System.out.println("手机内存是：" + p1.getMeonry());//三星
    }
}
```

### 5.避免修改接口实现一个类中两种同名方法的调用

一个类要继承一个类，并且要实现一个接口，如果这两个里面有一个相同的方法，可以使用内部类来实现接口，这样就不会与外围类的方法冲突

```java
//普通类
public class Father{
    public void Love(){
        System.out.println("FatherLove");
    }
}
//接口类
public interface Mother(){
    void Love();
}
//接口实现
public class Me extends Father{
    //子类方法1
    public void Love(){
        super.Love();
    } 
    //子类方法2
    public void MotherLove(){
        System.out.println("MotherLove");
    }
    //内部类实现接口
    public class meLove implements Mother{
        @Override
        public void Love(){
            MotherLove();
        }
    }
    //实现方法1
//    public melove getLove(){
//        return new meLove();
//    }
    
    //实现方法2
    public void getLove(){
        new meLove.Love();
    }
}
//主程序执行
Public class Main{
    public static void main(String[] args){
        Me me = new Me();
        me.Love();//FatherLove
//        me.getLove.Love();//MatherLove
        me.getLove();//MatherLove
    }
}
```

## 2. 内部类分类

### 1.非静态内部类和静态内部类

1. 在非静态内部类中不能定义静态成员
2. 在非静态的内部类中可以访问外部类的变量
3. 静态内部类中只可以访问静态成员而不可以访问非静态变量
4. 静态内部类中不可以访问外部类非静态的变量
5. 静态内部类的创建不依赖外部类，而非静态内部类的创建依赖外部类

```java
public class StaticInner{
    //非静态内部类
    public class InnerClass{
       void run() {
			System.out.println("走。。。");
		}
	}
    //静态内部类
	public static class staticInnerClass{
    	static void run() {
			System.out.println("跑。。。");
		}
	}
}

//实现方式
public static void main(String[] args){
    //非静态内部类的两种实现方式
    //实现方法 1
//    StaticInner staticInner = new StaticInner();
//    StaticInner.InnerClass innerClass = staticInner.new InnerClass();
    //实现方法 2
    StaticInner.InnerClass innerClass =new StaticInner().new InnerClass();
    
    //静态内部类的创建不依赖外部类，而非静态内部类的创建依赖外部类
    
    //静态内部类的实现方式 
    StaticInner.staticInnerClass statInnerClass = new StaticInner.staticInnerClass();
		staticInnerClass.run();
}
```

### 2.局部内部类

1. 局部内部类不允许访问修饰符修饰
2. 局部内部类对外完全隐藏，只能在局部使用
3. 主要应用在一些想要借助类来实现复杂工能，但是又不想这个类是公用的

- 局部内部类定义位置
  - 局部内部类是在方法中定义的类
- 局部内部类方式方式
  - 局部内部类，外界是无法直接使用，需要在方法内部创建对象并使用
  - 该类可以直接访问外部类的成员，也可以访问方法内的局部变量

```java
public class Outer {
	private String name = "宋天";
	private int age = 12;
	
	private void run() {
		System.out.println("跑。。");
	}

	public void functionClass() {
		//局部内部类不能使用任何访问修饰符
		class InnerClass{
			private void fun() {
				System.out.println("局部内部类方法");
				System.out.println(name);
				System.out.println(age);
			}
		}
		//局部内部类实现方法
		InnerClass innerClass = new InnerClass();
		innerClass.fun();
	}
	
	public static void main(String[] args) {
		Outer outer = new Outer();
		outer.functionClass();
	}
}

```

### 3.匿名内部类

1. 匿名内部类必须实现或继承一个已有的接口
2. 匿名内部类没有名字，也没有访问修饰符
3. 匿名内部类没有构造方法，因为匿名内部类没有名字

- 匿名内部类的前提

  - 存在一个类或者接口，这里的类可以是具体类也可以是抽象类

- 匿名内部类的格式

  - 格式：new 类名 ( ) {  重写方法 }    new  接口名 ( ) { 重写方法 }

  - 示例：

    ```java
    new Inter(){
        @Override
        public void method(){}
    } 
    ```

- 匿名内部类的本质

  - 本质：是一个继承了该类或者实现了该接口的子类匿名对象

- 匿名内部类的细节

  - 匿名内部类可以通过多态的形式接受

    ```java
    Inter i = new Inter(){
      @Override
        public void method(){
            
        }
    }
    ```

- 匿名内部类直接调用方法

  ```java
  interface Inter{
      void method();
  }
  
  class Test{
      public static void main(String[] args){
          new Inter(){
              @Override
              public void method(){
                  System.out.println("我是匿名内部类");
              }
          }.method();	// 直接调用方法
      }
  }
  ```

综合案例：

```java
public class Button {

	private void click() {
		new ActionListener() {
			@Override
			public void onAction() {
				// TODO Auto-generated method stub
				System.out.println("跑跑跑。。。。");
			}
			
		}.onAction();//执行
	}
	//匿名内部类没有名字
	//匿名内部类必须继承或实现一个已有的接口
	public interface ActionListener{
		public void onAction();
	}
	//主程序执行
	public static void main(String[] args) {
		//创建外部类
		Button button = new Button();
		button.click();
	}
	
}
```

**匿名内部类在开发中的使用（应用）**

- 当发现某个方法需要，接口或抽象类的子类对象，我们就可以传递一个匿名内部类过去，来简化传统的代码

示例代码：

```java
interface Jumpping {
    void jump();
}
class Cat implements Jumpping {
    @Override
    public void jump() {
        System.out.println("猫可以跳高了");
    }
}
class Dog implements Jumpping {
    @Override
    public void jump() {
        System.out.println("狗可以跳高了");
    }
}
class JumppingOperator {
    public void method(Jumpping j) { //new Cat();   new Dog();
        j.jump();
    }
}
class JumppingDemo {
    public static void main(String[] args) {
        //需求：创建接口操作类的对象，调用method方法
        JumppingOperator jo = new JumppingOperator();
        Jumpping j = new Cat();
        jo.method(j);

        Jumpping j2 = new Dog();
        jo.method(j2);
        System.out.println("--------");

        // 匿名内部类的简化
        jo.method(new Jumpping() {
            @Override
            public void jump() {
                System.out.println("猫可以跳高了");
            }
        });
		// 匿名内部类的简化
        jo.method(new Jumpping() {
            @Override
            public void jump() {
                System.out.println("狗可以跳高了");
            }
        });
    }
}
```



### 4.成员内部类（理解）

- 成员内部类的定义位置
  - 在类中方法，跟成员变量是一个位置
- 外界创建成员内部类格式
  - 格式：外部类名.内部类名 对象名 = 外部类对象.内部类对象;
  - 举例：Outer.Inner oi = new Outer().new Inner();
- 成员内部类的推荐使用方案
  - 将一个类，设计为内部类的目的，大多数都是不想让外界去访问，所以内部类的定义应该私有化，私有化之后，再提供一个可以让外界调用的方法，方法内部创建内部类对象并调用。

```java
class Outer {
    private int num = 10;
    private class Inner {
        public void show() {
            System.out.println(num);
        }
    }
    public void method() {
        Inner i = new Inner();
        i.show();
    }
}
public class InnerDemo {
    public static void main(String[] args) {
		//Outer.Inner oi = new Outer().new Inner();
		//oi.show();
        Outer o = new Outer();
        o.method();
    }
}
```



### 5.内部类的访问特点 

- 内部类可以直接访问外部类的成员，包括私有
- 外部类要访问内部类的成员，必须创建对象

示例

```java
/*
    内部类访问特点：
        内部类可以直接访问外部类的成员，包括私有
        外部类要访问内部类的成员，必须创建对象
 */
public class Outer {
    private int num = 10;
    public class Inner {
        public void show() {
            System.out.println(num);
        }
    }
    public void method() {
        Inner i = new Inner();
        i.show();
    }
}
```

## 3.参数传递

1. 类名作为形参和返回值（应用）

   - 类名作为方法的形参

     方法的形参是类名，其实需要的是该类的对象

     实际传递的是该对象的【地址值】

   - 类名作为方法的返回值

     方法的返回值是类名，其实返回的是该类的对象

     实际传递的，也是该对象的【地址值】

   - 示例

     ```java
     class Cat {
         public void eat() {
             System.out.println("猫吃鱼");
         }
     }
     class CatOperator {
         public void useCat(Cat c) { //Cat c = new Cat();
             c.eat();
         }
         public Cat getCat() {
             Cat c = new Cat();
             return c;
         }
     }
     public class CatDemo {
         public static void main(String[] args) {
             //创建操作类对象，并调用方法
             CatOperator co = new CatOperator();
             Cat c = new Cat();
             co.useCat(c);
     
             Cat c2 = co.getCat(); //new Cat()
             c2.eat();
         }
     }
     ```

2.  抽象类作为形参和返回值（理解）

   - 抽象类作为形参和返回值
     - 方法的形参是抽象类名，其实需要的是该抽象类的子类对象
     - 方法的返回值是抽象类名，其实返回的是该抽象类的子类对象

   - 示例：

   - ```java
     abstract class Animal {
         public abstract void eat();
     }
     class Cat extends Animal {
         @Override
         public void eat() {
             System.out.println("猫吃鱼");
         }
     }
     class AnimalOperator {
         public void useAnimal(Animal a) { //Animal a = new Cat();
             a.eat();
         }
         public Animal getAnimal() {
             Animal a = new Cat();
             return a;
         }
     }
     public class AnimalDemo {
         public static void main(String[] args) {
             //创建操作类对象，并调用方法
             AnimalOperator ao = new AnimalOperator();
             Animal a = new Cat();
             ao.useAnimal(a);
     
             Animal a2 = ao.getAnimal(); //new Cat()
             a2.eat();
         }
     }
     ```

3. 接口名作为形参和返回值（理解）

   - 接口作为形参和返回值
     - 方法的形参是接口名，其实需要的是该接口的实现类对象
     - 方法的返回值是接口名，其实返回的是该接口的实现类对象

   - 示例代码

     ```javascript
     interface Jumpping {
         void jump();
     }
     class JumppingOperator {
         public void useJumpping(Jumpping j) { //Jumpping j = new Cat();
             j.jump();
         }
         public Jumpping getJumpping() {
             Jumpping j = new Cat();
             return j;
         }
     }
     class Cat implements Jumpping {
         @Override
         public void jump() {
             System.out.println("猫可以跳高了");
         }
     }
     public class JumppingDemo {
         public static void main(String[] args) {
             //创建操作类对象，并调用方法
             JumppingOperator jo = new JumppingOperator();
             Jumpping j = new Cat();
             jo.useJumpping(j);
     
             Jumpping j2 = jo.getJumpping(); //new Cat()
             j2.jump();
         }
     }
     
     ```

     