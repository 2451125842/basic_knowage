# Java基础知识

## 一、概念辨析

**1 面向对象和面向过程的区别?**

- 面向过程:效率更高,但代码复用率低,难扩展,难维护,适用于单片机/嵌入式/Linux/Unix等面向过程开发
- 面向对象:易复用,易扩展,易维护,但是因为类需要实例化导致系统开销更大,效率更低.但对于大型软件开发来说,因为面向对象具有的封装、继承和多态,使我们能够设计出耦合度更低更灵活更易维护的系统

**2 解释型语言与编译型语言有什么区别?Java属于什么语言?**

* 解释型语言是说我们写成的代码并不直接翻译成二机制代码,而是翻译为一种中间语言,每次运行代码的时候由解释器边运行边翻译.效率较低,但可以实现跨平台
* 编译型语言就是我们写的代码直接编译成机器语言,计算机可以直接执行,效率较高,但是无法实现跨平台
* Java属于半编译语言,它将代码翻译为字节码也就是.class文件,由JVM来执行这些代码,因此Java在实现了跨平台的同时效率比一般的解释型语言更高

**3 Java的特点有哪些?**

- 面向对象(封装、继承、多态)

- 跨平台

- 编译与解释共存

- 支持多线程,而C++是不支持的,它需要调用操作系统的多线程功能来实现编程

**4 多态的具体实现有几种?**

​	两种多态,一个是继承过程中对方法的重写,一个是方法的重载.

**5 接口和抽象类的区别**

- 接口方法默认是 public，所有方法在接口中不能有实现，抽象类可以有非抽象的方法
- 接口中的实例变量默认是 final 类型的，而抽象类中则不一定
- 一个类可以实现多个接口，但最多只能实现一个抽象类
- 一个类实现接口的话要实现接口的所有方法，而抽象类不一定
- 接口不能用 new 实例化，但可以声明，但是必须引用一个实现该接口的对象 从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

**6 final修饰引用类型变量和基本类型变量有什么区别?**

 final修饰的基本类型变量变成了一个常量,大小不能改变;而修饰引用类型变量仅表示引用不变,或者说地址不变,其内属性还是可以改变的.

```java
//以下使用的类均为下文深浅克隆定义的类
public Main {
    public static void main(String[] args) {
      final Teacher t1 = new Teacher(new Student());
          t1.getStudent().setName("tiantian");
          System.out.println(t1.getStudent().getName());//输出结果为:tiantian
    }
}
```

**7 Object可被继承的方法都是什么?**

​	toString(), hashCode(), equals(), getClass(), clone(), finalize() 

**8 C语言的字符串有一个'\0'的结束符,Java有吗?如果没有,为什么没有?**

​	Java中String的底层实现是一个final char[] 数组,而每个数组都有一个length属性,因此我们可以很轻易的知道字符串的长度,而不需要浪费1个字节来使用哨兵来标示字符串结尾

*相关问题,String为什么是不可变的*

**9 StringBuilder和StringBuffer的区别是什么,相比于String,为什么他们可变?**

​	StringBuiler和StringBuffer都继承自AbstractStringBuiler,但StringBuilder线程不安全,StringBuffer加了同步锁,线程安全.

**10 Java中的IO流分为几种?他们之间有什么区别?**

- 按流向分,分为输入流和输出流
- 按操作单位,分为字节流和字符流
- 按流的角色,分为节点流和处理流

输入流是指从外部向当前对象输入流数据,输出则是当前对象向外输出数据流    

​	字节流操作的是字节数据,一般以Stream结尾的类都是字节流,他们都继承自InputStream和OutputStream;字符流操作的基本单位是字符,由JVM将字节流转化得到,以Reader和Writer结尾的类均为字符流,同时Reader和Writer也是他们的父类.InputStream、OutputStream、Reader、Writer均为抽象类

​	节点流是直接与数据源相连的流类型,而处理流则与包装后的节点流,它不会直接与数据源连接,可以通过缓冲的方式来提高IO吞吐量,可以便捷地一次输入/输出大量内容.

<p color=red>节点流和处理流的区分。 </p>

**11 有了字节流,我们为什么还要字符流?**

​	因为字节流转化成字符的过程非常耗时,而且因为编码类型的多样性很容易出现乱码,不如就由JVM直接提供一个操作字符流的接口

**12 如何接收键盘输入?**

​	使用Scanner类和BufferReader类

```java
Scanner scanner = new Scanner(System.in);
```

```java
BufferReader reader = new BufferReader(
  											new InputStreamReader(System.in));
```

**13 深克隆与浅克隆的实现**

​	浅克隆:new一个对象,复制对象内的所有基本类型数据,复制引用类型的数据引用并对引用类型内嵌套的引用类型执行浅克隆

​	深克隆:new一个对象,复制对象内所有的基本类型数据,对于引用类型数据,我们继续new一个对象然后将基本类型复制,然后对该引用类型内嵌套的引用类型继续执行深克隆

​	深克隆有两种实现方式,第一种方式如下:

```java
public class Test {
    public static void main(String[] args) {
        Teacher teacher1 = new Teacher(new Student()), teacher2 = teacher1.clone();

        System.out.println(teacher1==teacher2);
        System.out.println(teacher1.getStudent() == teacher2.getStudent());
    }
}

class Student implements Cloneable {
    private String name;

  	public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
  
    public Object clone() {
        Object student = null;
        try {
            student = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return student;
    }
}

class Teacher implements Cloneable {
    private String name;
    private Student student;

    public Student getStudent() {
        return student;
    }

    public Teacher(Student student) {
        this.student = student;
    }
    
    public Teacher clone() {
        Teacher res = null;
        try {
            res = (Teacher) super.clone();
            res.student = (Student) student.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return res;
    }
}
```

​	输出结果是:

​	false

​	false

​	证明实现了深克隆.这种方法必须手动将对象内的每一个引用类型数据都做一遍clone(),引用类型的clone()方法如果重写时漏掉了嵌套的引用类型就会失败,但这种方法的好处在于Object的clone()方法调用的是native方法,它是c实现的,速度更快.

```java
public class Test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Teacher teacher1 = new Teacher(new Student()), teacher2 = (Teacher) teacher1.deepClone();

        System.out.println(teacher1==teacher2);                             //输出为false
        System.out.println(teacher1.getStudent() == teacher2.getStudent()); //输出为false
    }
}

class Student implements Serializable {
    private static final long serialVersionUID = -2670549298404052141L;
    private String name;

		public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Teacher implements Serializable {
    private String name;
    private Student student;

    public Student getStudent() {
        return student;
    }

    public Teacher(Student student) {
        this.student = student;
    }
    
    public Object deepClone() throws IOException, ClassNotFoundException {
        ByteArrayOutputStream b = new ByteArrayOutputStream();
        ObjectOutputStream oo = new ObjectOutputStream(b);
        oo.writeObject(this);

        ByteArrayInputStream in = new ByteArrayInputStream(b.toByteArray());
        ObjectInputStream inputStream = new ObjectInputStream(in);
        return inputStream.readObject();

    }
}
```

​	在序列化的方法下,即使需要克隆的对象内的引用对象没有实现Cloneable接口,我们也可以完成深克隆.

​	浅克隆的实现比较简单,就是实现Cloneable接口,并在 public Object clone()方法中写return super.clone();就行了.当然,该抛异常抛异常.

## 二、容器/集合框架

### 2.1 概述

|        Collection接口        |        Map接口         |
| :--------------------------: | :--------------------: |
| Set接口、List接口、Queue接口 | AbstractMap、SortedMap |

Set接口下的实现类有HashSet、LinkedHashSet、TreeSet(TreeSet实现的是Set接口的子接口SortedSet，底层存储为红黑树)

List: ArrayList，Vector，LinkedList

Queue: LinkedList

Map: HashMap、TreeMap、HashTable(ConcurrentHashMap)

### 2.2 Set、List、Map三者的区别

- Set: 元素唯一, 无序,可以为null
- List: 元素不唯一, 有序
- Map: 使用键值对存储,key唯一,可以为null（除HashTable外）

### 2.3 ArrayList和LinkedList的区别

- 他们都是线程不安全的
- 从底层数据结构不同,ArrayList是Object数组,LinkedList是双向链表
- 由于数据结构的不同,自然添加/删除、遍历、随机存取、内存消耗就各不相同了
- ArrayList和线性表不同的一点在于它为长度变化预留了存储空间,这是它内存浪费的地方,LinkedList对于内存的浪费自然是因为它的指针了

### 2.4 ArrayList和Vector的区别

Vector线程安全,ArrayList线程不安全,当然Vector保证线程安全的同时使访问它花费的时间更多

### 2.5 ArrayList的扩容机制

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {//计算当前最小容量
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {//这个常量是new ArrayList时的空Object[]
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);//扩容函数
}
private void grow(int minCapacity) {//扩容为原来的1.5倍（取整），如果还不够，则使用上面得到的最小容量
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        		Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
}
```

初始化时，如果没有定义容量，则会定义一个Default Object数组，第一次添加时，如果数量不超过10，ArrayList new一个size为10的Object[]；如果超过，则new一个Object[最小容量]。然后将原先数组中的内容Arrays.copyOf过去。其后再次添加，就会调用上面的方法扩容。

补：LinkedList中get方法使用二分法的思想加速了一次，毕竟头尾指针双链链表，不过也只加速了这一次。

```java
Node<E> node(int index) {
    // assert isElementIndex(index);
    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

### 2.6 LRU怎么实现?

LRU是最近最少使用置换算法

使用LinkedHashMap<>()集合实现LRU,new的时候传入true让accessOrder为true就能实现

### 2.7 HashMap链表树化需要两个条件,其中一个是链表长度大于8,另一个是什么?

当HashMap每次插入数据的时候,会检查该链表的长度是否达到阀值8, 如果达到阀值, 那么检查数组容量是否小于MIN_TREEIFT_CAPACITY(=64), 如果小于64, 则进行扩容操作,如果不小于则对链表树化.

### 2.8 基础概念补充--哈希环

<img src="/Users/tianweinan/Library/Application Support/typora-user-images/image-20210402202519280.png" alt="image-20210402202519280" style="zoom:33%;" />

[国际惯例](https://baijiahao.baidu.com/s?id=1623504532153417105&wfr=spider&for=pc)

## 三、泛型

## 四、Java8新特性

​	Java8新特性非常多，本文主要讨论如下几个：1、Lambda表达式；2、方法引用；3、默认方法；4、新编译工具；5、Stream流；6、DateTime API；7、Option；8、Nashorn JavaScript引擎等 [参考链接](https://www.runoob.com/java/java8-new-features.html)

### 4.1 Lambda表达式

​	Lambda表达式又称闭包，在我看来是JS函数映射到JAVA的结果。我们通过闭包实现了一个方法中函数和实现的解耦 <font color=yellow size=2>个人理解</font>。但这种解耦合是有条件的，在定义的，需要被解耦合的接口中，只能有一个方法需要被实现，即可以有其他方法，但其他方法必须已实现。

#### 4.1.1 语法

```java
paramter -> expression;
(parameters) -> expression;
(parameters) -> {statements;}
```

​	参数可声明可省略，参数只有一个时可省略括号，语句只有一句时可省略大括号。

#### 4.1.2 Demo实例

```java
/**
 * @author shanhe
 * @className Main
 * @date 2021-04-13 19:21
 **/
public class Main<T> {

    public static void main(String[] args) {
        int num = 1;
        //num++;   lambda表达式调用的局部变量可不手动声明为final，但编译器会将其视为final
        Compare<Integer> compare = param -> System.out.println("=====first:" + (num+param));
        compare.compare(1);//输出2
        compare = param -> System.out.println("======next:"+(num-param));
        compare.compare(1);//输出1，实现解耦
    }
  
    public interface Compare<E> {
        void compare(int num);
    }
}
```

#### 4.1.3 扩展

​	我们可以使用Lambda表达式代替大多数匿名内部类，但是这种情况会消耗更多的计算资源。

​	主因是Lambda表达式映射为某个函数的实现时需要额外消耗资源。但当Lambda逻辑简单、循环次数大于1000时这种影响就已经很小了。因此我们在具体的工程中要灵活使用Lambda表达式。实验代码如下，[参考链接](https://blog.csdn.net/sinat_35322593/article/details/105219031)。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Consumer;

/**
 * @author shanhe
 * @className Main
 * @date 2021-04-13 19:21
 **/
public class Main<T> {

    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        for(int i = 0; i < 1000; i++) {
            list.add(i);
        }
      
        //不使用lambda表达式
        long starTime = System.currentTimeMillis();
        Consumer<Integer> consumer = new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {

            }
        };
        list.forEach(consumer);
        System.out.println("不使用Lambda表达式用时：" + (System.currentTimeMillis()-starTime) + "ms");

        //lambda表达式
        starTime = System.currentTimeMillis();
        list.forEach(param -> {
            //不做任何处理
        });
        System.out.println("使用了Lambda表达式用时：" + (System.currentTimeMillis()-starTime) + "ms");

    }
}
```

​	[最后再给一份简单的学习资料](https://www.cnblogs.com/haixiang/p/11029639.html)

### 4.2 方法引用

​	方法引用是函数与实现解耦方案的组成部分之一，lambda表达式解决了新实现的解耦，方法引用则将已实现的函数解耦，方便我们灵活调用。

#### 4.2.1 语法

```java
classVar  :: functionName         //对象::方法-函数引用
className :: staticFunctionName   //类名::静态方法-函数引用
className :: functionName         //类名::方法-函数引用
```

#### 4.2.2 Demo实例

```java
public class Car {
        private int num;
  
        public Car() {
        }
        public Car(int i) {
            this.num = i;
        }
  
        public static void collide(final Car car) {
            System.out.println("carNum : " + car.num);
        }

        public static void collide(final Car car1, final Car car2) {
            System.out.println("default");
        }

        public void carInfo() {
            System.out.println("Info:" + this.toString());
        }

        @Override
        public String toString() {
            return "Car{" +
                    "num=" + num +
                    '}';
        }
}    
    @Test
    public void TestFunctionCite() {
        Supplier<Car> supplierCar = Car::new;//构造器引用
        Car car = supplierCar.get();
        List<Car> cars = Arrays.asList(car);//嗯，尽量还是别用asList，这东西太坑
        cars.forEach(Car::collide);
        supplierCar = () -> new Car(1);     //lambda实现函数引用
        car = supplierCar.get();
        cars = Arrays.asList(car);
        cars.forEach(Car::collide);         //从这里可以看出lambda表达式的函数“赋值”和函数引用的“赋值”无本质区别

        List<Car> list = new ArrayList<>();
        list.add(car);
        list.add(new Car(-1));
        list.add(new Car(-2));
        list.forEach(Car::carInfo);         //类名::方法-函数引用

        Consumer<String> consumer = System.out::print;//对象::方法-函数引用
        consumer.accept("123");

        Function<Integer, String[]> function = String[]::new;
        String[] apply = function.apply(20);
    }
```

​	对于重写的函数，我们通过接口定义的变量来唯一确定一个方法。

### 4.3 函数式接口



## 五、并发

### 3.1 并发的实现

并发有三种实现方式,两种通过实现接口,一种通过继承Thread类

接口的实现方式是一种线程任务,继承的实现方式是创建一个多线程本身,此时的线程与任务耦合在一起,如果我们需要多个线程执行相同的任务那么就需要写很多相同的代码,但是当我们将线程任务与线程分离、解耦就能够实现线程任务的复用

具体代码如下:

```java
class MyThread1 implements Runnable {
    public void run() {
				System.out.println("我的线程1");
    }
}

/**
	Runnable无返回值,无法抛出异常,而Callable接口可以,具体如下:
*/
class MyThread2 implements Callable<T> {
  	public T call() throws Exception {
      	System.out.println("我的线程2");
    }
  
}

class MyThread3 extends Thread {
  	public void run() {
      	System.out.println("我的线程3");
    }
}

public class Test {
  	public static void main(String[] args) {
      	MyThread1 t1 = new MyThread1();
      	MyThread2 t2 = new Mythread2();
      	MyThread3 t3 = new MyThread3();
      
      	//用继承实现的多线程类直接调用start()方法创建新线程
      	t3.start();
      
      	//接口实现的多线程需要将势力传入Thread实例中再调用start()
      	Thread thrad1 = new Thread(t1);
      	thread1.start();
      
      	//有返回值的需要封装到FutureTask类中再传入Thread实例
      	FutureTask<T> task = new FutureTask<>(t2);
      	Thread thread2 = new Thread(task);
      	thread2.start();
    }
}
```

- 继承实现与接口实现的比较:

  继承Thread类开销过大,而且Java不支持多重继承,但可以实现多个接口,因此接口实现更灵活;但继承Thread实现多线程的代码更简单,同时这种方法的两个实例天然无线程安全问题,但如果我们将同一个Runnable/Callable<>实例传入多个线程中,却会出现线程安全问题.

## JVM

### 1. 内存区域

### 2. 垃圾回收算法

### 3. 类的加载机制

### 4. 对象的创建过程



## Java锁机制

### 1. synchronized

#### 1.1 synchronized底层原理

#### 1.2 synchronized机制

##### 1.2.1 偏向锁

##### 1.2.2 轻度锁

##### 1.2.3 锁自旋

##### 1.2.4 重度锁

### 2. volatile

### 3. ThreadLocal



## 常见的算法问题

### 1. 现有100G的数据,数据为多行字符串,每行为一个url且可重复,现给出1G内存空间,需要求出重复次数最多的前100个url

首先将100G数据读入内存,使用字符流,每次读取0.8G,对每个url取hashCode,然后新建5000个文件,对每个url的hashCode取余,余数相同的url存入相同的文件中,这样就保证了相同的url在同一个文件中.

然后逐个读取5000个文件,对于每个文件: 使用HashMap计算每个url的数量,取keySet转化为ArrayList,使用Collections工具按每个key的value值重排序,排序算法为堆排序获取重复数最多的前100个url,将这100个url以键值对的方式重新写回文件中.

对5000个文件两两归并,每次取前100个url写入新文件中,原来的两个文件删除,归并算法为有序链表合并问题的算法,双指针遍历即可

### 2. 成环问题及寻找环起点

快慢指针法判断是否成环.

快指针走过的节点数-慢指针节点数=环节点数,然后快指针先走一个环节点数,慢指针从head处开始走,快慢指针指向同一个节点时,该节点为起点