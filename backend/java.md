1. 你觉得 Java 好在哪儿？

首先 Java 是跨平台的，不同平台执行的机器码是不一样的，而 Java 因为加了一层中间层 JVM ，所以可以做到一次编写多平台运行，即 「Write once,Run anywhere」。

编译执行过程是先把 Java 源代码编译成字节码，字节码再由 JVM 解释或 JIT 编译执行，而因为 JIT 编译时需要预热的，所以还提供了 AOT（Ahead-of-Time Compilation），可以直接把字节码转成机器码，来让程序重启之后能迅速拉满战斗力。（解释执行比编译执行效率差）

Java 还提供垃圾自动回收功能，虽说手动管理内存意味着自由、精细化地掌控，但是很容易出错。

在内存较充裕的当下，将内存的管理交给 GC （Carbage Collection）来做，减轻了程序员编程的负担，提升了开发效率，更加划算！

GC详细参考博客：https://blog.csdn.net/suifeng3051/article/details/48292193

现在 Java 生态圈有丰富的第三方类库、网上全面的资料、企业级框架、各种中间件等等，总之你要的都有。

2. 反射用过吗？反射的原理


3. 讲一下垃圾回收算法


4. 什么时候使用LinkedList?


5. 如果让你设计一个 HashMap 如何设计？

https://winnerchen.github.io/yiheng.github.io/2017/08/26/%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AAHashMap/
https://www.huaweicloud.com/articles/498753d943d56991cb9c985732be66e7.html
LeetCode 706: https://leetcode-cn.com/problems/design-hashmap/




6. 能说下类加载过程吗


7. jvm如何调优，参数怎么调？如何利用工具分析jvm状态？


8. 线程和进程的区别


