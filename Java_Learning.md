### 第一章 类与对象

#### 1.1 面向对象

###### 面向对象三个主要特征：

>	-  封装性：内部的操作对外部而言不可见，当内部的操作都不可直接使用的时候才是安全的；
>	-  继承性：在已有结构的基础上，继续进行功能的扩充；
>	-  多态性：是在继承性的基础上扩充而来的概念,指得是类型的转换处理





#### 1.2 类与对象

>类：类是对某一类实物的共性的抽象概念。
>
>对象：对象描述的是一个具体的产物。		
>
>两者之间关系：对象是从类中实例化出来的例子。





#### 1.3 对象内存分析

Java之中类属于<font color = CadetBlue>**引用数据类型**</font>，引用数据类型最大的困难之处在于要进行内存的管理，同时在操作的时候也会发生有内存关系的一个变化。

>- 堆内存：存储对象的具体信息（属性，方法等等）
>- 栈内存：存储分配出来的内存的地址（储存的是指向内存的指针）

```java
// 类的定义
class Person{
	String name;
    int age;
    public void tell{
    	System.out.println("姓名: "+name);
        System.out.println("年龄: "+age);
    }
}

// 声明并实例化对象
Person p = new Person();
p.name = "张三";
p.age = 20;	
// 进行方法的调用
p.tell(); 	

//	 1.new 开辟了新的堆内存, new拥有开辟内存的最高级别
// 	 2.开辟的内存内容是要根据Person类来进行初始化的
```

<font color=orange>注意：</font>所有的对象在调用类中的属性或方法的时候必须要**<font color = CadetBlue>实例化</font>**完成后才可以执行。





#### 1.4 对象引用分析

​	1.4.1 引用传递分析

​			类本身属于引用数据类型，既然是引用数据类型，那就牵扯到内存的引用传递，所谓的引用传递的本质：**同一块堆内存空间，可以被不同的栈内存指向，也可以更换指向。（可以当成C指针）**

> <font color = pink>**例子：**</font>
>
> ```java
> public class Demo {
>     public static void main(String[] args){
>         Person p1  = new Person();
>         p1.name = "zhangsan";
>         p1.age = 20;
>         p1.tell();
>         System.out.println("-----------p2------------");
>         Person p2 = p1; // 引用传递
>         p2.age = 80;
>         p2.tell();
>         System.out.println("-----------p1------------");
>         p1.tell();
>     }
> }
> 
> class Person{
>     String name;
>     int age;
>     public void tell(){
>         System.out.println("姓名 ： "+ name);
>         System.out.println("年龄 ： "+ age);
> 
>     }
> }
> ```

>**Person p2 = p1;** 
>p1 代表一个地址，比如0x0001
>**p2  =  p1；** 	
>代表p2也等于 0x0001，指向堆空间中同一块内存
>**p2.age = 80；**
>修改了该堆空间中的数值，这时，p1本身数值没有改变仍是0x0001，也就是说，仍然指向着这块堆空间（已经被修改）所以再次调用**p1.tell();** 后，输出 age也为80。
>
>
>
>><font color = pink>**输出结果：**</font>
>>
>>```java
>>姓名 ： zhangsan
>>年龄 ： 20
>>--------------------p2--------------------------
>>姓名 ： zhangsan
>>年龄 ： 80
>>--------------------p1--------------------------
>>姓名 ： zhangsan
>>年龄 ： 80
>>```





#### 1.5 引用与垃圾产生分析

**垃圾：**没有任何栈内存指向的堆内存空间，所有的垃圾将被GC（垃圾收集器，Garbage Collector）不定期进行回收并释放无用内存空间，但是如果垃圾过多，一定影响到GC的处理性能，从而降低整体的程序性能，所以在开发过程中，垃圾产生应该越少越好。

#### 1.6.1 成员属性封装处理

在类之中的组成就是属性和方法，一般而言方法都是对外提供服务的，所以不需要封装，而属性需要较高的安全性，往往需要对其进行保护，这时候就需要采用封装性对属性进行保护。



> <font color =CadetBlue>**属性封装方法：**</font><font color= red>（类中所有属性都必须使用private封装！！！）</font>
>
> - 1. 修改属性的可见性来限制对属性的访问（一般限制为private），例如：
>
>     ```java
>     public class Person {
>         private String name;
>         private int age;
>     }
>     ```
>
>     这段代码中，将 **name** 和 **age** 属性设置为私有的，只能本类才能访问，其他类都访问不了，如此就对信息进行了隐藏。
>
> 
>
> - 2. 【getter】、【setter】设置：对每个值属性提供对外的公共方法访问，也就是创建一对赋取值方法，用于对**<font color  = red>私有属性(private)</font>**的访问，一定要配合使用，否则外部还是能通过直接引用来修改属性，这就违背了使用setter和getter的意义
>
>         例子：
>   
>         ```java
>         public class Demo {
>           public static void main(String[] args) {
>                 Person p1 = new Person();
>                 p1.setName("张三");
>                 p1.setAge(19);
>               p1.tell();
>                 //p1.name = "李四"；
>                 /*
>                 这时这里已经无法直接对name属性进行修改了，
>               会报错：'name' 在 'com.Person' 中具有 private 访问权限
>                 */
>             }
>         }
>       
>         
>         class Person {
>             /**
>              * 思考：
>              * 虽然我们使用了getter和setter进行类的封装，但是在调用方法上，依然可以使用.name这样的
>              * 语句进行直接调用，有没有像python中 ._name这种形式使得在外部无法调用.name呢？
>              *
>              * 答案是有的，在Java中我们只要使用【private】权限修饰符修饰我们希望封装的对象属性即可。
>              */
>         
>             private String name;
>             private int age;
>         
>             /**
>              * 使用setter和getter方法
>              */
>             public void setName(String name) {
>                 this.name = name;
>             }
>         
>             public String getName() {
>                 return this.name;
>             }
>         
>             public void setAge(int age) {
>                 this.age = age;
>             }
>         
>             public int getAge() {
>                 return this.age;
>             }
>         
>             public void tell() {
>                 System.out.println("姓名" + this.name);
>                 System.out.println("年龄" + this.age);
>             }
>         
>         }
>         ```
>
> 

#### 1.6.2 **this**有三种用法：

1. 当前类中的属性：**this**.属性名

> **使用this 调用当前类中属性** 
>
> - <font color= orange>代码第6行和第10行：</font>**this**关键字是为了解决实例变量（private String name）和局部变量（setName(String name)中的name变量）之间发生的同名的冲突。
> - <font color=red>只要访问本类中的属性，一定加上**this**进行属性访问，养成好习惯</font>



2. 当前类中的方法（普通方法、构造方法）：**this**.方法名()、**this**（)

> - 构造方法调用（**this()**）：使用关键字new实例化对象时才会调用构造方法
> - 普通方法调用（**this.方法名称()**）: 利用重构和this() 来提高代码重用
> - <font color = red>**构造方法必须在实例化新对象时调用，因此this()的语句只允许放在【构造方法】的【首行】**</font>
> - 构造方法互相调用时，要注意别形成死循环。

> <font color = pink>**例子：**</font>
>
> ```java
> public class Demo {
>     public static void main(String[] args) {
>         Person p1 = new Person();
>         System.out.println("--------【使用getter、setter】--------");
>         System.out.println("--------【预期结果：张三  19】--------");
>         p1.setName("张三");
>         p1.setAge(19);
>         p1.tell();
> 
>         System.out.println("--------【使用构造方法】--------");
>         System.out.println("--------【预期结果： 李四  28 】--------");
>         Person p2 = new Person("李四", 28);
>         p2.tell();
> 
>         System.out.println("--------【使用this() 调用构造方法】--------");
>         System.out.println("--------【预期结果： 打印两遍“我是一个构造函数~”  null  8  王五  80 】--------");
>         // 构造方法只能在实例初始化的时候使用
>         Person p3 = new Person(8);
>         Person p4 = new Person("王五",80);
> 
>         p3.tell();
>         p4.tell();
>     }
> }
> 
> 
> class Person {
>     /**
>      * 思考：
>      * 虽然我们使用了getter和setter进行类的封装，但是在调用方法上，依然可以使用.name这样的
>      * 语句进行直接调用，有没有像python中 ._name这种方法使得在外部无法使用.name方法呢？
>      * <p>
>      * 答案是有的，在Java中我们只要使用private权限修饰符就可以了
>      */
> 
>     private String name;
>     private int age;
> 
>     /**
>      * 使用setter和getter方法
>      */
>     public void setName(String name) {
>         this.name = name;
>     }
> 
>     public String getName() {
>         return this.name;
>     }
> 
>     public void setAge(int age) {
>         this.age = age;
>     }
> 
>     public int getAge() {
>         return this.age;
>     }
> 
>     /**
>      * 用this()方法，把构造函数和重构结合，减少代码重用
>      */
> //    public Person() {
> //        System.out.println("我是一个构造函数~");
> //    }
> //
> //    public Person(int age) {
> //        System.out.println("我是一个构造函数~");
> //        this.age = age;
> //    }
> 
>     /**
>      *  观察上方两个构造函数，发现有的部分是重复的代码，为了减少重复，
>      *  可以使用 this() 来调用构造方法
>      */
>     
>     //在java类中，如果不显示声明构造函数，JVM 会给该类一个默认的构造函数。一个类可以有多个构造函数。
>     // 构造函数的主要作用:
>     // 一是用来实例化该类。
>     // 二是让该类实例化的时候执行哪些方法，初始化哪些属性
>     // 当一个类声明了构造函数以后，JVM 是不会再给该类分配默认的构造函数。
>     
>     public Person() {
>         System.out.println("我是一个构造函数~");
>     }
> 
>     public Person(int age) {
>         // 调用无参构造函数
>         this();
>         this.age = age;
>     }
> 
>     /**
>      * 构造函数: 可以解决参数过多，需要设置一大堆getter、setter的问题
>      */
>     public Person(String name, int age) {
>         //调用单参构造函数
>         this(age);
>         this.name = name;
>     }
> 
>     public void tell() {
>         System.out.println("姓名" + this.name);
>         System.out.println("年龄" + this.age);
>     }
> 
> }
> 
> ```
>
> <font color = pink>**输出结果：**</font>
>
> ```java
> 我是一个构造函数~
> --------【使用getter、setter】--------
> --------【预期结果：张三  19】--------
> 姓名张三
> 年龄19
> --------【使用构造方法】--------
> --------【预期结果： 李四  28 】--------
> 我是一个构造函数~
> 姓名李四
> 年龄28
> --------【使用this() 调用构造方法】--------
> --------【预期结果： 打印两遍“我是一个构造函数~”  null  8  王五  80 】--------
> 我是一个构造函数~
> 我是一个构造函数~
> 姓名null
> 年龄8
> 姓名王五
> 年龄80
> ```



3. 描述当前对象

> .。。。。。。。。。。

#### 1.7 构造方法与匿名对象

如果一个类中有较多的属性，那么用全部调用setter方法实在是太繁琐了，为了解决这个问题，Java中专门提供有构造方法，即，<font color=CadetBlue>**可以通过构造方法实现实例化对象中的属性初始化处理。**</font>

> Java中构造方法的定义要求如下：
>
> - 方法名称必须与类名称保持一致；
>
> - 构造方法不允许设置任何返回值类型，也就是说，没有返回值定义。
>
> - 构造方法是在使用关键字new实例化对象的时候自动调用的。
>
>     ><font color = pink>**例子：**</font>
>     >
>     >```java
>     >public class Demo {
>     >    public static void main(String[] args) {
>     >        Person p1 = new Person("", 0);
>     >        System.out.println("-------------【无封装👇】------------");
>     >        p1.age = 20;
>     >        p1.name = "zhangsan";
>     >        p1.tell();
>     >        System.out.println("-------------【构造函数👇】----------");
>     >        //构造方法是在使用关键字new实例化对象的时候自动调用的。
>     >        Person p2 = new Person("lisi", 19);
>     >        p2.tell();
>     >
>     >    }
>     >}
>     >
>     >class Person {
>     >    String name;
>     >    int age;
>     >
>     >    public Person(String n, int a) {
>     >      //方法名称必须与类名称保持一致；
>     >      name = n;
>     >      age = a;
>     >
>     >    }
>     >
>     >    public void tell() {
>     >      System.out.println("姓名 ： " + name);
>     >      System.out.println("年龄 ： " + age);
>     >    }
>     >}
>     >```
>     >
>     >
>     >
>     >**<font color = orange>注意：</font>**
>     >
>     >第20行处，构造方法不允许设置任何返回值类型，也就是说，没有返回值定义,也不用void！因为这里如果有类型定义，那么构造方法就与普通函数方法结构完全相同了，这样编译器会认为此方法是一个普通方法。
>     ><font color = red>两者的最大**区别**就在于：构造方法是在类对象实例化（new的时候）的时候调用的
>     >而普通方法是在类对象实例化完成之后调用的（new之后）</font>
>     >
>     ><font color = pink>**输出结果：**</font>
>     >
>     >```java
>     >----------------------【无封装👇】-------------------------
>     >姓名 ： zhangsan
>     >年龄 ： 20
>     >----------------------【构造函数👇】-----------------------
>     >姓名 ： lisi
>     >年龄 ： 19
>     >```

#### 1.8 简单Java类<font color = orange>（超重要）</font>

所谓的简单Java类指的是可以描述某一类信息的程序类，例如：描述一个人、描述一本书，并且在这个类中并没有特别复杂的逻辑操作。只作为一种信息存储的媒介存在。

对于简单Java类而言，其核心开发结构如下：

	- 类名称一定要有意义，可以明确的描述某一类事物。
 - 类之中所有属性都必须使用private进行封装，同时封装后的属性必须要提供有getter、setter方法。
 - <font color = red>类之中可以提供无数多个构造方法，但是必须要保留有无参构造方法。</font>建议写出一个无参构造在加上一个包含所有参数的构造方法，因为如果不显示的写出无参构造方法，而写出带参的构造方法，默认的无参构造方法就会被覆盖。
 - 类之中不能有任何输出语句，所有内容的获取必须返回。
 - 可以提供一个获取对象详细信息的方法，暂时将此方法名称定义为 **getInfo**() ；

<font color = pink>**例子：**</font>

```java
/**
 * 现在假设有这样一个要求，定义一个雇员类，该类会包含雇员编号、姓名、职位、基本工资几个属性信息。
 *
 * @author Keendy
 */
public class SimpleClass {
    public static void main(String[] args) {
        System.out.println(new Emp(10124563, "Keendy", "Engineer", 3300.00).getInfo());
    }
}

class Emp {
    private int serial;
    private String name;
    private String position;
    private double salary;

    public void setSerial(int serial) {
        this.serial = serial;
    }

    public int getSerial() {
        return this.serial;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public void setPosition(String position) {
        this.position = position;
    }

    public String getPosition() {
        return this.position;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return this.salary;
    }

    /**
     * 无参构造
     */
    public Emp() {
    }

    /**
     * 有参构造
     */

    public Emp(int serial, String name, String position, double salary) {
        this.serial = serial;
        this.name = name;
        this.position = position;
        this.salary = salary;
    }


    public String getInfo() {
        return "编号：" + this.serial + '\n' + "姓名：" + this.name + "\n" + "职位：" + this.position + "\n" + "薪水：" + this.salary;
    }
}
```

