# JIT
Java 字节码是解释执行的，但是没有直接在 JVM 宿主执行原生代码快。为了提高性能，Oracle Hotspot 虚拟机会找到执行最频繁的字节码片段并把它们编译成原生机器码。编译出的原生机器码被存储在非堆内存的代码缓存中。通过这种方法，Hotspot 虚拟机将权衡下面两种时间消耗：将字节码编译成本地代码需要的额外时间和解释执行字节码消耗更多的时间。

![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/7/4/JVM_JIT_interraction.png)

此问题起源于在一次性能测试过程中，为了模拟有大量超时的情况，使用eclipse debug住服务器代码，然后使用jmeter 脚本施加压力。在这个过程当中，发现了一个很有趣的现象，就是性能测试计划执行十分钟，前三分钟左右时确实返回的是超时，平均响应时间在500ms以上， 但是突然一下子就降到了8 ms，并且接下来的七分钟都是这个样子的。对这种现象感觉得奇怪，然后就去问了下大牛，解释说是Java JIT。回来后google 了几篇文章，发现JIT 还是个很复杂的玩意儿，所以在这先简单记下，后续再细化相关功能。
JIT 是Just-In-Time Compiliation 的缩写，中文为即时编译。就是JAVA在运行过程中，如果有些动态极度频繁的被执行或者不被执行，就会被自动编译成机器码，跳过其中的部分环节。
Java源码通过编译器转为平台无关的字节码（bytecode）或Java class文件。
在启动Java应用程序后，JVM会在运行时加载编译后的类并通过Java解释器执行适当的 语义计算。
 当开启JIT时，JVM会分析Java应用程序的函数调用并且（达到内部一些阀值后）编译字节码为本地更高效的机器码。JIT流程通常为最繁忙的函数调用提供更高的优先级。
一旦函数调用被转为机器码，JVM会直接执行而不是“解释执行”。
上述过程会在程序运行过程中不断提高性能。
