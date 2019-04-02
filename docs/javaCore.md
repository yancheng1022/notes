[1. Lock和synchronized的区别？](#1)

[2. 锁的分类](#2)

[3. wait（）和sleep（）的区别？](#3)

[4. 深克隆和浅克隆的区别？](#4)

[5. java访问权限修饰符作用范围？](#5)

[6. hashcode() 和equals()关系？](#6)

[7. 类加载过程？](#7)

[8. 内存溢出和内存泄漏？](#8)

[9. 谈谈jvm内存结构？](#9)

[10.谈谈java内存模型(JMM)](#10)

[11.谈谈volatile关键字？](#11)

[12.判断对象死亡的两种算法？](#12)

[13. 垃圾回收算法有哪些？](#13)

[14.老年代中的对象是怎么来的？](#14)

[15.常见的垃圾收集器？](#15)

[16. java跨平台原理？](#16)

[17. hashmap原理？](#17)

[18. 死锁必要条件？](#18)

[19. String s = new String("xyz")创建了几个对象？](#19)

[20. 实现多线程的方式？](#20)

[21. 顺序打印A B C](#21)

[22. ThreadLocal原理？](#22)

### <span id="1">1. Lock和synchronized的区别?</span>

​	（1）synchronized遇到异常时自动释放锁，lock不会（所以需要在finally中lock.unlock释放锁）

​	（2）lock可以指定等待时间（lock.trylock(5,TimeUnit.SECOND)）

​	（3）synchronized是java关键字 lock是一个类



### <span id="2">2.锁的分类？</span>

​	（1）可重入锁：一个synchronized方法调用另外一个synchronized方法时，不需要重新申请锁（synchronized ReentLock）

​	（2）可中断锁：获取锁的过程可以被中断（lock 是可中断的lockInterrutibly（） synchronized不可被中断）

​	（3）公平锁：以请求顺序获取锁 等待时间最久的线程获得锁（synchronized是非公平锁，ReentrantReadWriteLock 默认非公平锁，但可设为公平锁new reentlock（true））

​	（4）读写锁：将对一个资源的访问分成两个锁，一个读锁，一个写锁 通过readLock（）获取读锁，writeLock（）获取写锁

​	（5）悲观锁：每次拿数据的时候认为别人都会修改，所以都会上锁（synchronized）

​	（6）乐观锁：每次拿数据的时候认为别人不会修改，所以不上锁（concurrnet.automic就是使用了乐观锁的一种实现方式CAS实现的）CAS：多个线程同时操作一个变量时，只有一个线程能修改这个变量，其他线程修改失败并不被挂起，而是可以继续尝试。



### <span id="3">3. wait()和sleep()的区别？</span>

​	（1）sleep（）是Thread类的方法，wait（）是Object类的方法

​	（2）sleep（）不释放锁对象，wait（）释放锁对象

​	（3）wiat（） notify（）只能在同步方法或同步代码块中使用，sleep（）可以在任何地方使用



### <span id="4">4. 浅克隆和深克隆的区别？</span>

​	（1）浅克隆：创建一个新对象，对于非基本类型属性，仍指向原属性所指向的内存地址

​	（2）深克隆：创建一个新对象，非基本类型属性也不指向原属性指向的内存地址

​	（clone（）是浅克隆，如何实现深克隆？实现cloneable接口，重写clone（），让它的引用类型属性调用clone（）方法）



### <span id="5">5. java访问控制修饰符作用范围？</span>

​	public：项目中的任何一个类都能访问

​	protected：同包和不同包下的子类都能访问

​	default：同一包下的类可以访问

​	private：私有类 其他类不能访问



### <span id="6">6. hashcode()和equals()的区别？</span>

​	(1)equals()相等，hashcode()必相等

​	(2)hashcode()相等，equals()不一定相等

​	（这样做可以提高容器的查询效率。equals（）方法比hashcode（）方法效率低，如果都用equals（）比较，这样效率很低。比如hashmap，可以先通过hashcode（）确定桶的位置，然后通过equals（）方法比较桶中的元素，这样能提高效率）



### <span id="7">7. 类加载过程？</span>

​	（1）加载：在内存中生成一个代表这个类的class对象

​	（2）验证：确保class文件中的字节流包含的信息符合当前虚拟机的需求

​	（3）准备：正式为类变量分配内存并设置类变量的初始值

​	（4）解析：将常量池中的符号引用替换为直接引用

​	（5）初始化：执行类构造器方法的过程



### <span id="8">8.内存溢出和内存泄漏区别</span>

​	内存溢出：程序在申请内存时，没有足够的空间供其使用 （eg：代码出现死循环，out of memory）

​	内存泄漏：分配出去的内存不再使用，但是无法回收（eg：File Stream等资源没有及时释放）



### <span id="9">9. 谈谈jvm内存结构？</span>

​	jvm分为线程共享区和线程独占区

​	线程独占区（java栈 本地方法栈 程序计数器）

​	线程共享区（堆和方法区）

​	（1）java栈：存放栈帧，栈帧的压栈对应着方法的调用。栈帧中包括局部变量表，操作数栈，方法引用，返回地址等

​	（2）本地方法栈：作用与java栈类似，只是本地方法栈执行native方法

​	（3）程序计数器：保存程序当前执行的指令地址

​	（4）堆：用来存储对象和数组

​	（5）方法区：存储类信息，静态变量，常量以及编译后的代码



### <span id="10">10. 谈谈java内存模型(JMM)？</span>

​	java memory model —— java内存模型，是java线程间通信的机制

​	线程之间共享变量存储在主内存，每个线程都有自己的本地内存，存储的是共享变量在本地的副本



### <span id="11">11.谈谈volatile关键字？</span>

​	一旦一个共享变量（类成员变量，静态成员变量）被volatile修饰后，具备了两层语义：

​	（1）保证不同线程对这个变量操作时的可见性（怎么保证？volatile会强制将修改值立即写入主存，并且使其它线程的工作内存中该值无效，需要重新从主存读取）

​	（2）禁止进行指令重排序

​	（注意：volatile不能保证原子性，因为修改值和写入主内存不是一个原子操作）



### <span id="12">12. 判断对象死亡的两种算法？</span>

​	（1）引用计数法：给对象添加一个引用计数器，当有一个地方引用它时，计数器值加1，引用失效值就减1。计数器值为0时对象不能再被使用（缺点：循环引用）

​	（2）可达性分析法（jvm采用）:设立若干根对象，当任何一个根对象到某一个对象均不可达时，认为这个对象可以被回收



### <span id="13">13. 垃圾回收算法有哪些？</span>

​	（1）标记-清除算法：通过根节点，标记所有根节点开始的可达对象，清除未被标记对象（会产生内存碎片）

​	（2）复制算法（新生代GC）：将内存分为一块较大的Eden和两块较小的survivor，每次使用Eden和其中一块survivor。Gc时将标记的对象复制到另一块survivor

​	（3）标记-整理算法（老年代gc）：将标记的对象移动到内存的一端，清除边界外的所有空间

​	（4）分代收集算法：把java堆分为新生代和老年代（新生代存活率低。采用复制算法。老年代存活率高，采用标记整理算法）



### <span id="14">14. 老年代中的对象是怎么来的？</span>

​	（1）15次young gc后还没被回收的对象进入老年代

​	（2）大对象直接进入老年代



### <span id="15">15. 常见的垃圾收集器？</span>

​	（1）Serial收集器：采用一条收集线程完成收集工作。收集时必须暂停其它线程工作

​	（2）ParNew收集器：serial收集器的多线程版本

​	（3）G1收集器：对垃圾回收进行了划分优先级的操作

​	（4）CMS收集器（老年代收集器）：上面三种都会stop the world，cms不会。。是以最短停顿时间为目标的收集器（过程：初始标记 并发标记 重新标记 并发清除）



### <span id="16">16. java跨平台原理？</span>

​	.java文件编译成.class字节码文件

​	然后在各平台只需要安装对应的jvm，就能将字节码转换为机器码，运行

​	从而实现了一次编译，到处运行



### <span id="17">17.  hashmap原理？</span>

​	Hashmap用来存储键值对。底层数据结构是Entry数组+链表，两个重要的方法put（）和get（）。Put时根据hash算法确定桶的位置，再根据equals方法判断在链表中的位置，有的话替换，没有就添加到链表尾。Get时通过hash算法得到桶的位置，在调用equals方法确定在链表中的位置，最后返回值

补充：（1）如果桶中链表记录过大（超过8）就会把这个链表动态变成红黑二叉树，查询复杂度O(n)变成O(logn)

​	    （2）Hashmap条目超过加载因子（0.75）*当前容量（初始16）时进行扩容





### <span id="18">18. 死锁必要条件？</span>

​	（1）互斥使用，即当资源被一个线程使用(占有)时，别的线程不能使用 

​	（2）不可抢占，资源请求者不能强制从资源占有者手中夺取资源，资源只能由资源占有者主动释放。 

​	（3）请求和保持，即当资源请求者在请求其他的资源的同时保持对原有资源的占有。 

​	（4）循环等待，即存在一个等待队列：P1占有P2的资源，P2占有P3的资源，P3占有P1的资源。这样就形成了一个等待环路。 

​	

### <span id="19">19. String     s= new    String(   "xyz   ")创建了几个对象？</span>

​	  一个或两个

​	字符串常量池如果没有xyz  就创建， 然后堆上new string("xyz")----共两个

​	如果字符串常量池中有xyz，只在对上创建对象-----共一个



#### <span id="20">20. 实现多线程的方式？</span>

1.继承Thread类，重写run方法

2.实现Runnable接口，实现run方法

3.通常为了简便第二中写法，通常用匿名内部类

```java
Thread a = new Thread(new Runnable() {
	            public void run() {
	                System.out.println(Thread.currentThread().getName() + "A");
	            }
	     });
//new Runnable（）不要理解为实现接口
```



#### <span id="21">21. 顺序打印A B C</span>   	

（1）方式一：newSingleThreadExecutor

```java
/**
 * 我们要做的就是有三个线程a，b，c，这三个线程都有一个方法分别打印A、B、C，问怎么能实现依次打印ABC的功能
 * @author zhmm
 */
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        Thread a = new Thread(new Runnable(){
            public void run() {
                System.out.println("a");
            }
        });
        Thread b = new Thread(new Runnable(){
            public void run() {
                System.out.println("b");
            }
        });
        Thread c = new Thread(new Runnable(){
            public void run() {
                System.out.println("c");
            }
        });
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        executorService.submit(a);
        executorService.submit(b);
        executorService.submit(c);
        executorService.shutdown();
    }
}

```

（2）方式二.join方法

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread a = new Thread(new Runnable(){
            public void run() {
                System.out.println("a");
            }
        });
        Thread b = new Thread(new Runnable(){
            public void run() {
                System.out.println("b");
            }
        });
        Thread c = new Thread(new Runnable(){
            public void run() {
                System.out.println("c");
            }
        });
        a.start();
        //抢占main方法时间片，直到a执行完才继续执行
        a.join();
        b.start();
        b.join();
        c.start();
        c.join();

    }
}

```



#### <span id="22">22. 交替打印A B C？</span>

（1）方法一：利用wait notify flag

```java
public class Main {
    public static void main(String[] args) {
        final Print p = new Print();
        new Thread(new Runnable(){

            public void run() {
                while (true){
                    try {
                        p.a();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
        new Thread(new Runnable(){

            public void run() {
                while (true){
                    try {
                        p.b();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
        new Thread(new Runnable(){

            public void run() {
                while (true){
                    try {
                        p.c();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
    }
}


class Print{
    private int flag = 1;
    public void a() throws InterruptedException {
        synchronized (this){
            while (flag != 1){
                this.wait();
            }
            System.out.println("a");
            flag = 2;
            this.notifyAll();
        }
    }
    public void b() throws InterruptedException {
        synchronized (this){
            while (flag != 2){
                this.wait();
            }
            System.out.println("b");
            flag = 3;
            this.notifyAll();
        }
    }
    public void c() throws InterruptedException {
        synchronized (this){
            while (flag != 3){
                this.wait();
            }
            System.out.println("c");
            flag = 1;
            this.notifyAll();
        }
    }
}
```



（2）利用lock打印

```
/**
 * 我们要做的就是有三个线程a，b，c，这三个线程都有一个方法分别打印A、B、C，问怎么能实现依次打印ABC的功能
 * @author zhmm
 */
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Main {
    private static Lock lock = new ReentrantLock();//通过JDK5中的Lock锁来保证线程的访问的互斥
    private static int state = 0;

    static class ThreadA extends Thread {
        @Override
        public void run() {
            while (true) {
                try {
                    lock.lock();
                    while (state % 3 == 0) {//多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.println("A");
                        state++;
                    }
                } finally {
                    lock.unlock();// lock()和unlock()操作结合try/catch使用
                }
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            while (true) {
                try {
                    lock.lock();
                    while (state % 3 == 1) {//多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.println("B");
                        state++;

                    }
                } finally {
                    lock.unlock();// lock()和unlock()操作结合try/catch使用
                }
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            while (true){
                try {
                    lock.lock();
                    while (state % 3 == 2) {//多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.println("C");
                        state++;
                    }
                } finally {
                    lock.unlock();// lock()和unlock()操作结合try/catch使用
                }
            }
        }
    }

    public static void main(String[] args) {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}

```



#### <span id="22">22. ThreadLocal原理？</span>

ThreadLocal,本地线程变量，用来存储线程的本地变量。底层实际上是用map容器来存储变量，key是当前线程，value是要存储的值













































