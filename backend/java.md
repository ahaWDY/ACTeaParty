1. 你觉得 Java 好在哪儿？

首先 Java 是跨平台的，不同平台执行的机器码是不一样的，而 Java 因为加了一层中间层 JVM ，所以可以做到一次编写多平台运行，即 「Write once,Run anywhere」。

编译执行过程是先把 Java 源代码编译成字节码，字节码再由 JVM 解释或 JIT 编译执行，而因为 JIT 编译时需要预热的，所以还提供了 AOT（Ahead-of-Time Compilation），可以直接把字节码转成机器码，来让程序重启之后能迅速拉满战斗力。（解释执行比编译执行效率差）

Java 还提供垃圾自动回收功能，虽说手动管理内存意味着自由、精细化地掌控，但是很容易出错。

在内存较充裕的当下，将内存的管理交给 GC （Carbage Collection）来做，减轻了程序员编程的负担，提升了开发效率，更加划算！

GC详细参考博客：https://blog.csdn.net/suifeng3051/article/details/48292193

现在 Java 生态圈有丰富的第三方类库、网上全面的资料、企业级框架、各种中间件等等，总之你要的都有。

2. 反射用过吗？反射的原理

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


3. 讲一下垃圾回收算法


4. 什么时候使用LinkedList?


5. 如果让你设计一个 HashMap 如何设计？

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


6. 能说下类加载过程吗


7. jvm如何调优，参数怎么调？如何利用工具分析jvm状态？


8. 线程和进程的区别


