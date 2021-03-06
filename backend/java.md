## 1. 你觉得 Java 好在哪儿？

首先 Java 是跨平台的，不同平台执行的机器码是不一样的，而 Java 因为加了一层中间层 JVM ，所以可以做到一次编写多平台运行，即 「Write once, Run anywhere」。

编译执行过程是先把 Java 源代码编译成字节码，字节码再由 JVM 解释或 JIT 编译执行，而因为 JIT 编译时需要预热的，所以还提供了 AOT（Ahead-of-Time Compilation），可以直接把字节码转成机器码，来让程序重启之后能迅速拉满战斗力。（解释执行比编译执行效率差）

Java 还提供垃圾自动回收功能，虽说手动管理内存意味着自由、精细化地掌控，但是很容易出错。

在内存较充裕的当下，将内存的管理交给 GC （Carbage Collection）来做，减轻了程序员编程的负担，提升了开发效率，更加划算！

GC详细参考博客：https://blog.csdn.net/suifeng3051/article/details/48292193

现在 Java 生态圈有丰富的第三方类库、网上全面的资料、企业级框架、各种中间件等等，总之你要的都有。

## 2. 反射用过吗？反射的原理

参考博客：https://blog.csdn.net/m0_57541165/article/details/117922838?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-4.no_search_link&spm=1001.2101.3001.4242

反射就是程序运行状态中，对于任意一个类，能够知道这个类的所有属性和方法，对于任意一个对象，能够调用方法/获取属性。

作用：对于在编译期无法确定使用哪个数据类的场景，通过反射可以在程序运行时构造出不同的数据类实例

原理：Java在编译之后会生成一个class文件，反射通过字节码文件找到其类中的方法和属性等

反射常见的应用场景：

Spring 实例化对象：当程序启动时，Spring 会读取配置文件applicationContext.xml并解析出里面所有的 标签实例化到IOC容器中。
反射 + 工厂模式：通过反射消除工厂中的多个分支，如果需要生产新的类，无需关注工厂类，工厂类可以应对各种新增的类，反射可以使得程序更加健壮。
JDBC连接数据库：使用JDBC连接数据库时，指定连接数据库的驱动类时用到反射加载驱动类

优点：增加程序的灵活性：面对需求变更时，可以灵活地实例化不同对象

缺点：破坏类的封装性：可以强制访问 private 修饰的信息；性能损耗：反射相比直接实例化对象、调用方法、访问变量，中间需要非常多的检查步骤和解析步骤，JVM无法对它们优化


## 3. 讲一下垃圾回收算法

详细参考博客：https://juejin.cn/post/6981812825735987208#heading-15

垃圾是指在运行程序中没有任何指针指向的对象，这个对象就是需要被回收的垃圾。如果不及时对内存中的垃圾进行清理，那么，这些垃圾对象所占的内存空间会一直保留到应用程序结束，被保留的空间无法被其他对象使用。甚至可能导致内存溢出。

**怎么确定是垃圾**

**引用计数法**

给对象添加一个引用计数器，每当有一个地方引用它时，计数器加一。反之每当一个引用失效时，计数器减一。当计数器为0时，则表示对象不被引用。

**可达性分析**

设立若干根对象（GC Root），每个对象都是一个子节点，当一个对象找不到根时，就认为该对象不可达。

**标记-清除算法**

该算法先标记，后清除，遍历所有的GC Root，分别标记处可达的对象和不可达的对象，然后将不可达的对象回收。

这种算法的不足主要体现在效率和空间

从效率的角度讲，标记和清除两个过程的效率都不高,因为需要遍历所有GC ROOT.

从空间的角度讲，标记清除后会产生大量不连续的内存碎片， 内存碎片太多可能会导致以后程序运行过程中在需要分配较大对象时，无法找到足够的连续内存而不得不提前触发一次垃圾收集动作。

**复制算法**

将内存分为两块，每次只使用一块。当这一块内存满了，就将还存活的对象复制到另一块上，并且严格按照内存地址排列，然后把已使用的那块内存统一回收。

优点是：解决了标记-清除算法的碎片问题,能够得到连续的内存空间
缺点是：浪费了一半内存

**标记整理算法**

复制算法在对象存活率较高时,持续复制效率非常低,老年代都是不易被回收的对象,针对老年代的特点,可以采用标记整理算法,标记整理算法在标记-清除算法基础上,它标记之后,不直接清理,是让所有存活对象都向一端移动，然后直接清理掉边界以外的内存.这样既避免了对象存活率较高时的持续复制,也避免了内存碎片的出现.适用于老年代.

**分代收集算法**

现代商用虚拟机基本都采用分代收集算法来进行垃圾回收。这种算法没什么特别之处，就是上面内容的结合.

它是对内存中的对象按照生命周期的长短,以及所在区域的不同进行划分.

堆区分为新生代,老年代.

方法区为持久代.

新生代就是活不了多久就死亡的对象，一般在堆内存.比如局部变量.

老年代是活的久但也会死亡的对象，一般在堆内存.比如一些生命周期长的对象.

持久代是不死的对象，一般在方法区.比如加载的class信息.

不同的年代使用不同的垃圾回收算法.

新生代使用复制算法.

老年代使用标记整理算法.

方法区的垃圾回收

持久代也就是方法区,Java虚拟机规范中说过可以不要求虚拟机在方法区实现垃圾收集，因为和堆区的垃圾回收效率相比，它的回收效率实在太低，但是此部分内存区域也是可以被回收的。

持久代和堆区的新生代和老年代不一样.方法区主要回收的内容是废弃常量和无用的类。

对于废弃常量也可通过引用的可达性来判断，但是对于无用的类则需要同时满足下面3个条件：

- 该类所有的实例都已经被回收，也就是Java堆中不存在该类的任何实例；
- 加载该类的ClassLoader已经被回收；
- 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。.


## 4. 什么时候使用LinkedList?

总的来说：

    当操作是在一列 数据的后面添加数据而不是在前面或中间,并且需要随机地访问其中的元素时,使用ArrayList会提供比较好的性能；

    当你的操作是在一列数据的前面或中 间添加或删除数据,并且按照顺序访问其中的元素时,就应该使用LinkedList了。

ArrayList和LinkedList在性能上各 有优缺点，都有各自所适用的地方，总的说来可以描述如下：

1．对ArrayList和LinkedList而言，在列表末尾增加一个元素所花的开销都是固定的。

对 ArrayList而言，主要是在内部数组中增加一项，指向所添加的元素，偶尔可能会导致对数组重新进行分配；

而对LinkedList而言，这个开销是 统一的，分配一个内部Entry对象。

2．在ArrayList的 中间插入或删除一个元素意味着这个列表中剩余的元素都会被移动；

而在LinkedList的中间插入或删除一个元素的开销是固定的。

3．LinkedList不 支持高效的随机元素访问。

4．ArrayList的空 间浪费主要体现在在list列表的结尾预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗相当的空间


## 5. 如果让你设计一个 HashMap 如何设计？

自己动手实现一个HashMap： https://winnerchen.github.io/yiheng.github.io/2017/08/26/%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AAHashMap/

HashMap是怎么实现的？手写一个HashMap： https://www.huaweicloud.com/articles/498753d943d56991cb9c985732be66e7.html

LeetCode 706: https://leetcode-cn.com/problems/design-hashmap/

大致分为以下几个步骤：
- Map接口：HashMap的顶层接口
  - 需要包含put, get, size方法
  - 一个内部接口Entry<K, V>（内部接口包含getKey()和getValue()两个方法）
- 内部类Entry：把内部接口Entry<K, V>实现为内部类
- 成员变量
  - 默认数组长度
  - 默认负载因子
  - Entry数组
  - HashMap的大小
- 构造器
- 哈希函数：哈希函数将任意长度的key转化为固定长度的index（散列值）。优秀的哈希算法应该保证散列值均匀，并且尽量避免冲突（不同的数据有同样的散列值）。下面列出几种，其余可以参考Hash算法原理：https://blog.csdn.net/tanggao1314/article/details/51457585
  - **生成散列值**
    - 平方取中法：H(key) = key^2 的中间几位
    - 基数转换法：变换基底
    - 除留余数法：哈希表长为m，p为小于等于m的最大素数，H(key) = key % p
  - 避免冲突
    - 开放定址法：使用再散列方法调整散列值
      - 线性探测再散列
      - 二次探测再散列
      - 伪随机探测再散列
    - 再哈希法：同时构造多个不同的哈希函数，如果冲突就使用下一个不同的哈希函数
    - 链地址法（使用**链表**）：哈希地址为i的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第i个单元中
- put方法：使用哈希函数算出index之后，用“链表”的方式把新元素添加到这个index对应的链表中去
- HashMap扩容：当map的大小大于默认长度 * 默认负载因子，那么数组的**长度会翻倍**，数组中的数据会**重新散列**然后再存放。重新散列往往是因为长度改变了，所以散列值也会发生改变。在扩容方法写好之后，需要加到put方法中调用。
- get方法：首先把key进行散列得到index，之后找到index位置的链表，再逐一比较找到真正的entry
- size方法


## 6. 能说下类加载过程吗

推荐：https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/jvm/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E8%BF%87%E7%A8%8B.md

https://blog.csdn.net/javazejian/article/details/73413292

一个类的完整生命周期包括以下几步
- 加载
- 链接（其中包括验证、准备、解析三步）
- 初始化
- 使用
- 卸载

类加载过程包括其中前三步，也就是加载、链接、初始化。类加载过程是指将Class类型文件加载到虚拟机中的过程。加载和链接的部分内容是交叉进行的

(1) 加载

- 通过包名和类名，获取类文件的二进制字节流
- 把二进制字节流映射成JVM内方法区运行时数据结构
- 在虚拟机内存中，生成一个`java.lang.Class`对象，作为这个类的访问入口。

系统类加载器：classpath路径，开发者可以直接使用系统类加载器，一般情况下该类加载是程序中默认的类加载器

双亲委派模型：双亲委派模式要求除了顶层的启动类加载器外，其余的类加载器都应当有自己的父类加载器，请注意双亲委派模式中的父子关系并非通常所说的类继承关系，而是采用组合关系来复用父类加载器的相关代码

(2) 链接

- 验证
  - 文件格式验证
  - 元数据验证
  - 字节码验证
  - 符号引用验证
- 准备：为类变量分配内存并设置初始值
- 解析：虚拟机将常量池内的符号引用替换为直接引用的过程

(3) 初始化

执行构造器`init()`；如果该类具有父类，则优先保证父类`init()`先执行，再执行子类`init()`

## 7. jvm如何调优，参数怎么调？如何利用工具分析jvm状态？

参数篇：https://smartan123.github.io/book/?file=001-%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/002-%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88/0023-JVM%E8%B0%83%E4%BC%98-%E5%8F%82%E6%95%B0%E7%AF%87

工具篇：
https://www.cnblogs.com/chiangchou/p/jvm-4.html

https://blog.csdn.net/weixin_33759269/article/details/92055380

**参数**
- -server或者-client设置jvm的运行参数
  - Server VM 的初始堆空间会大一些，默认使用的是并行垃圾回收器，启动慢运行快。
  - Client VM相对来讲会保守一些，初始堆空间会小一些，使用串行的垃圾回收器，它的目标是为了让JVM的启动速度更快，但运行速度会比Serverm模式慢些。
  - JVM在启动的时候会根据硬件和操作系统自动选择使用Server还是Client类型的JVM
- -XX参数用于jvm的调优和debug操作
  - boolean类型：例如-XX:+DisableExplicitGC 表示禁用手动调用gc操作，也就是说调用 System.gc()无效
  - 非boolean类型：例如-XX:NewRatio=1 表示新生代和老年代的比值
- -Xms与-Xmx
  - -Xms设置jvm堆内存的初始大小
  - -Xmx设置jvm堆内存的最大大小

Java 8的堆内存模型
![1590290219261](https://user-images.githubusercontent.com/26801257/132763829-ed959fe8-db5c-4cdc-be9d-1910523c7f67.png)
- Young年轻区
- Tenured年老区
- MetaData本地内存空间

JVM监控分析工具一般分为两类
- JDK自带的工具调优：在jdk bin目录下面，以exe的形式直接点击就可以使用，其中包含分析工具已经很强大，几乎涉及了方方面面，但是我们最常使用的只有两款：jvisualvm.exe和jconsole.exe
  - VisualVM 是一个工具，它提供了一个可视界面，用于查看 Java 虚拟机 (Java Virtual Machine, JVM) 上运行的基于 Java 技术的应用程序（Java 应用程序）的详细信息。
  - jconsole直接在jdk/bin目录下点击jconsole.exe即可启动。在弹出的框中可以选择本机的监控本机的java应用，也可以选择远程的java服务来监控，如果监控远程服务需要在tomcat启动脚本中添加如下代码。jconsole概览图和主要的功能：概述、内存、线程、类、VM、MBeans等
```
-Dcom.sun.management.jmxremote.port=6969
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.authenticate=false
``` 
- 第三方分析工具：有很多，各自的侧重点不同
  - MAT(Memory Analyzer Tool)，基于Eclipse的内存分析工具
  - GCViewer
  - GCeasy
  - FastThread

## 8. 线程和进程的区别

https://www.huaweicloud.com/articles/d90c9bf248c8f1731946d65786c95379.html

https://www.zhihu.com/question/25532384

详细版本：https://blog.csdn.net/ThinkWon/article/details/102021274

一句话：进程圈起一部分内存空间，执行一段代码，关注资源的管理；而线程则在进程内部着重关注CPU运行，也可以拓展为多线程

- 根本区别：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位
- 资源开销：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。
- 包含关系：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。
- 内存分配：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的
- 影响关系：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。
- 执行过程：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行

## 9. java 容器都有哪些？

推荐博客：https://zhuanlan.zhihu.com/p/94312830

Java容器是一个Java所编写的程序。

    容器可以管理对象的生命周期、对象与对象之间的依赖关系。

    您可以使用一个配置文件（通常是XML），在上面定义好对象的名称、如何产生（Prototype 方式或Singleton 方式）、哪个对象产生之后必须设定成为某个对象的属性等，在启动容器之后，所有的对象都可以直接取用，不用编写任何一行程序代码来产生对象，或是建立对象与对象之间的依赖关系。

Java 容器分为 Collection 和 Map 两大类，其下又有很多子类，如下所示：

- Collection：List，ArrayList，LinkedList，Vector，Stack，Set，HashSet，LinkedHashSet，TreeSet

- Map：HashMap，LinkedHashMap，TreeMap，ConcurrentHashMap，Hashtable


## 10. jsp 和 servlet 有什么区别？

知乎回答：https://www.zhihu.com/question/37962386
菜鸟教程：https://www.runoob.com/jsp/jsp-intro.html


- JSP (Java Server Pages) 用JSP标签在HTML网页中插入Java代码，标签以<%开头，以%>结束。HTML和Java代码共同存在。Servlet输出HTML非常困难，JSP就是替代Servlet输出HTML的。JSP本身也是一种Servlet，JSP第一次被访问是会被编译为HttpJspPage类（该类是HttpServlet的一个子类）。
- Servlet其实就是一个遵循Servlet开发的Java类。Servlet是由服务器调用的，运行在服务器端。主要为了实现网上实现聊天、发帖这样的一些交互功能。

区别：
- Servlet在Java代码中通过HttpServletResponse对象动态输出HTML内容；JSP在静态HTML内容中嵌入Java代码，Java代码被动态执行后生成HTML内容
- Servlet能够很好地组织业务逻辑代码，但是在Java源文件中通过字符串拼接的方式生成动态HTML内容，需要处理大量的println语句，导致代码维护困难，可读性差；JSP虽然规避了Servlet在生成HTML内容方面的劣势，但是也会混入大量、复杂的业务逻辑。
- 可以通过MVC(Model-View-Controller)模式双剑合璧，在Controller/业务部分主要使用Servlet，在View/生成方面使用JSP
    - Web浏览器发送HTTP请求到服务端，被Controller(Servlet)获取并进行处理（例如参数解析、请求转发）
    - Controller(Servlet)调用核心业务逻辑也就是Model部分，获得结果
    - Controller(Servlet)将逻辑处理结果交给View（JSP），动态输出HTML内容
    - 动态生成的HTML内容返回到浏览器显示

## 11. java中哪些集合类是线程安全的？

早在jdk的1.1版本中，所有的集合都是线程安全的。但是在1.2以及之后的版本中就出现了一些线程不安全的集合，为什么版本升级会出现一些线程不安全的集合呢？因为线程不安全的集合普遍比线程安全的集合效率高的多。随着业务的发展，特别是在web应用中，为了提高用户体验减少用户的等待时间，页面响应速度(也就是效率)是优先考虑的。而且对线程不安全的集合加锁以后也能达到安全的效果(但是效率会低，因为会有锁的获取以及等待)。其实在jdk源码中相同效果的集合线程安全的比线程不安全的就多了一个同步机制，但是效率上却低了不止一点点，因为效率低，所以已经不太建议使用了。下面举一些常用的功能相同却线程安全和不安全的集合。

Vector：就比Arraylist多了个同步化机制（线程安全）。

Hashtable：就比Hashmap多了个线程安全。

ConcurrentHashMap:是一种高效但是线程安全的集合。

Stack：栈，也是线程安全的，继承于Vector。


## 12. java中创建线程有哪几种方式？

参考：https://segmentfault.com/a/1190000022878543

- 定义子类，继承Thread类创建线程类，调用start()启动线程；Java只能单继承，所以该线程不能继承其他类
- 实现Runnable接口，创建线程类，调用start()启动线程；只是实现了接口，还可以继承其他父类
- 通过Callable和Future创建线程；只是实现了接口，还可以继承其他父类
    - （1）创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，并且有返回值。
    - （2）创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值。
    - （3）使用FutureTask对象作为Thread对象的target创建并启动新线程。
    - （4）调用FutureTask对象的get()方法来获得子线程执行结束后的返回值。

Runnable和Callable的区别
 - (1) Callable规定（重写）的方法是call()，Runnable规定（重写）的方法是run()。
 - (2) Callable的任务执行后可返回值，而Runnable的任务是不能返回值的。
 - (3) call方法可以抛出异常，run方法不可以。
 - (4) 运行Callable任务可以拿到一个Future对象，表示异步计算的结果。它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。通过Future对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果。

## 13. Java中什么是单例？

菜鸟教程：https://www.runoob.com/design-pattern/singleton-pattern.html

单例保证一个类只有一个实例，并提供一个访问它的全局访问点。主要解决一个全局使用的类频繁创建与销毁的问题。

### 懒汉式（不推荐使用）

- 线程不安全：不能多线程使用
```{java}
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
  
    public static Singleton getInstance() {  
    if (instance == null) {  
        instance = new Singleton();  
    }  
    return instance;  
    }  
}
```

- 线程安全：加锁影响效率
```{java}
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
    public static synchronized Singleton getInstance() {  
    if (instance == null) {  
        instance = new Singleton();  
    }  
    return instance;  
    }  
}
```

### 饿汉式

- 容易产生垃圾对象
```{java}
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton() {}  
    public static Singleton getInstance() {  
    return instance;  
    }  
}
```

### 登记式/静态内部类

```{java}
public class Singleton {  
    private static class SingletonHolder {  
    private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
    return SingletonHolder.INSTANCE;  
    }  
}
```

### DCL

- 实现比较复杂，更推荐登记式

```{java}
public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
    if (singleton == null) {  
        synchronized (Singleton.class) {  
        if (singleton == null) {  
            singleton = new Singleton();  
        }  
        }  
    }  
    return singleton;  
    }  
}
```

### 枚举

- 最佳方法，但很少用

```{java}
public enum Singleton {  
    INSTANCE;  
    public void whateverMethod() {  
    }  
}
```

## 14. 什么是Java序列化？什么情况下需要序列化？

https://zhuanlan.zhihu.com/p/40301488

- 序列化：将Java对象转换成（二进制）字节流的过程
- 反序列化：将（二进制）字节流转换成Java对象的过程

当Java对象需要在网络上传输或者持久化存储到文件中时，就需要对Java对象进行序列化处理。例如，在常见的web服务中的持久化session就是一个很好的例子。一般session都是入驻内存的，当服务器异常宕机，内存里的session因为掉电而擦除，当我们设置了session持久化特性时，就会把session保存在硬盘上，这就是序列化。等服务器重启后又可以读取硬盘上这个session文件，还原session对象，这就是反序列化。

https://zhuanlan.zhihu.com/p/76045548

### 注意事项
1. 某个类可以被序列化，则其子类也可以被序列化
2. 声明为static和transient的成员变量，不能被序列化。static成员变量是描述类级别的属性，transient表示临时数据
3. 反序列化读取序列化对象的顺序要保持一致

序列化的实现
- 类实现Serializable接口，这个接口没有需要实现的方法。实现Serializable接口是为了告诉JVM这个类的对象可以被序列化。
- 实现Externalizable接口，接口中定义了两个方法writeExternal和readExternal
- ObjectOutputStream
- ObjectInputStream






