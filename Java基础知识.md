# Java基础知识

## 概念/语法

1. **面向对象和面向过程的区别?**

   - 面向过程:效率更高,但代码复用率低,难扩展,难维护,适用于单片机/嵌入式/Linux/Unix等面向过程开发
   - 面向对象:易复用,易扩展,易维护,但是因为类需要实例化导致系统开销更大,效率更低.但对于大型软件开发来说,因为面向对象具有的封装、继承和多态,使我们能够设计出耦合度更低更灵活更易维护的系统

2. **解释型语言与编译型语言有什么区别?Java属于什么语言?**

   * 解释型语言是说我们写成的代码并不直接翻译成二机制代码,而是翻译为一种中间语言,每次运行代码的时候由解释器边运行边翻译.效率较低,但可以实现跨平台
   * 编译型语言就是我们写的代码直接编译成机器语言,计算机可以直接执行,效率较高,但是无法实现跨平台
   * Java属于半编译语言,它将代码翻译为字节码也就是.class文件,由JVM来执行这些代码,因此Java在实现了跨平台的同时效率比一般的解释型语言更高

3. **Java的特点有哪些?**

   面向对象(封装、继承、多态)

   跨平台

   编译与解释共存

   支持多线程,而C++是不支持的,它需要调用操作系统的多线程功能来实现编程

4. **多态有几种?**

   两种多态,一个是继承过程中对方法的重写,一个是方法的重载.

5. **接口和抽象类的区别**

   - 接口方法默认是 public，所有方法在接口中不能有实现，抽象类可以有非抽象的方法
   - 接口中的实例变量默认是 final 类型的，而抽象类中则不一定
   - 一个类可以实现多个接口，但最多只能实现一个抽象类
   - 一个类实现接口的话要实现接口的所有方法，而抽象类不一定
   - 接口不能用 new 实例化，但可以声明，但是必须引用一个实现该接口的对象 从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

6. **final修饰引用类型变量和基本类型变量有什么区别?**

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

   

7. **Object可被继承的方法都是什么?**

   toString(), hashCode(), equals(), getClass(), clone(), finalize() 

8. **C语言的字符串有一个'\0'的结束符,Java有吗?如果没有,为什么没有?**

   Java中String的底层实现是一个final char[] 数组,而每个数组都有一个length属性,因此我们可以很轻易的知道字符串的长度,而不需要浪费1个字节来使用哨兵来标示字符串结尾

   *相关问题,String为什么是不可变的*

9. **StringBuilder和StringBuffer的区别是什么,相比于String,为什么他们可变?**

   StringBuiler和StringBuffer都继承自AbstractStringBuiler,但StringBuilder线程不安全,StringBuffer加了同步锁,线程安全.

10. **Java中的IO流分为几种?他们之间有什么区别?**

    - 按流向分,分为输入流和输出流
    - 按操作单位,分为字节流和字符流
    - 按流的角色,分为节点流和处理流

    输入流是指从外部向当前对象输入流数据,输出则是当前对象向外输出数据流

    字节流操作的是字节数据,一般以Stream结尾的类都是字节流,他们都继承自InputStream和OutputStream;字符流操作的基本单位是字符,由JVM将字节流转化得到,以Reader和Writer结尾的类均为字符流,同时Reader和Writer也是他们的父类.InputStream、OutputStream、Reader、Writer均为抽象类

    节点流是直接与数据源相连的流类型,而处理流则与包装后的节点流,它不会直接与数据源连接,可以通过缓冲的方式来提高IO吞吐量,可以便捷地一次输入/输出大量内容.

    <p color=red>节点流和处理流的区分。 </p>

11. **有了字节流,我们为什么还要字符流?**

    因为字节流转化成字符的过程非常耗时,而且因为编码类型的多样性很容易出现乱码,不如就由JVM直接提供一个操作字符流的接口

12. **如何接收键盘输入?**

    使用Scanner类和BufferReader类

    ```java
    Scanner scanner = new Scanner(System.in);
    ```

    ```java
    BufferReader reader = new BufferReader(
      											new InputStreamReader(System.in));
    ```

    

13. **深克隆与浅克隆的实现**

    浅克隆:new一个对象,复制对象内的所有基本类型数据,复制引用类型的数据引用并对引用类型内嵌套的引用类型执行浅克隆

    深克隆:new一个对象,复制对象内所有的基本类型数据,对于引用类型数据,我们继续new一个对象然后将基本类型复制,然后对该引用类型内嵌套的引用类型继续执行深克隆

    深克隆有两种实现方式,第一种方式如下:

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

    输出结果是:

    false

    false

    证明实现了深克隆.这种方法必须手动将对象内的每一个引用类型数据都做一遍clone(),引用类型的clone()方法如果重写时漏掉了嵌套的引用类型就会失败,但这种方法的好处在于Object的clone()方法调用的是native方法,它是c实现的,速度更快.

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

    在序列化的方法下,即使需要克隆的对象内的引用对象没有实现Cloneable接口,我们也可以完成深克隆.

    浅克隆的实现比较简单,就是实现Cloneable接口,并在 public Object clone()方法中写return super.clone();就行了.当然,该抛异常抛异常.

14. 



## 容器

### 1. 概述

|        Collection接口        |        Map接口         |
| :--------------------------: | :--------------------: |
| Set接口、List接口、Queue接口 | AbstractMap、SortedMap |

Set接口下的实现类有HashSet、LinkedHashSet、TreeSet(TreeSet实现的是Set接口的子接口SortedSet)

List: ArrayList，Vector，LinkedList

Queue: LinkedList

Map: HashMap、TreeMap、HashTable(ConcurrentHashMap)

### 2. Set、List、Map三者的区别

- Set: 元素唯一, 无序,可以为null
- List: 元素不唯一, 有序
- Map: 使用键值对存储,key唯一,可以为null

### 3. ArrayList和LinkedList的区别

- 他们都是线程不安全的
- 从底层数据结构不同,ArrayList是Object数组,LinkedList是双向链表
- 由于数据结构的不同,自然添加/删除、遍历、随机存取、内存消耗就各不相同了
- ArrayList和线性表不同的一点在于它为长度变化预留了存储空间,这是它内存浪费的地方,LinkedList对于内存的浪费自然是因为它的指针了

### 4. ArrayList和Vector的区别

Vector线程安全,ArrayList线程不安全,当然Vector保证线程安全的同时使访问它花费的时间更多

### 5. ArrayList的扩容机制

### 6. LRU怎么实现?

LRU是最近最少使用置换算法

使用LinkedHashMap<>()集合实现LRU,new的时候传入true让accessOrder为true就能实现

### 7.HashMap树化需要两个条件,其中一个是链表长度大于8,另一个是什么?

当HashMap每次插入数据的时候,会检查该链表的长度是否达到阀值8, 如果达到阀值, 那么检查数组容量是否小于MIN_TREEIFT_CAPACITY(=64), 如果小于64, 则进行扩容操作,如果不小于则对链表树化.



## 并发

### 1. 并发的实现

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

