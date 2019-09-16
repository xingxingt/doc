
#### 拼多多、饿了么、蚂蚁金服、百度等公司给我留下较深印象的一些java面试题：
* private修饰的方法可以通过反射访问，那么private的意义是什么
* Java类初始化顺序
* 对方法区和永久区的理解以及它们之间的关系
* 一个java文件有3个类，编译后有几个class文件
* 局部变量使用前需要显式地赋值，否则编译通过不了，为什么这么设计
* ReadWriteLock读写之间互斥吗
* Semaphore拿到执行权的线程之间是否互斥
* 写一个你认为最好的单例模式
* B树和B+树是解决什么样的问题的，怎样演化过来，之间区别
* 写一个生产者消费者模式
* 写一个死锁
* cpu 100%怎样定位
* String a = "ab"; String b = "a" + "b"; a == b 是否相等，为什么


------------------------------------------------------------------------------------------
#### 阿里一面题目：
* osi七层网络模型，五层网络模型，每次层分别有哪些协议
* 死锁产生的条件， 以及如何避免死锁，银行家算法，产生死锁后如何解决
* 如何判断链表有环
* 虚拟机类加载机制，双亲委派模型，以及为什么要实现双亲委派模型
* 虚拟机调优参数
* 拆箱装箱的原理
* JVM垃圾回收算法
* CMS G1
* hashset和hashmap的区别，haspmap的底层实现put操作，扩容机制，currenthashmap如何解决线程安全,1.7版本以及1.8版本的不同
* md5加密的原理
* 有多少种方法可以让线程阻塞，能说多少说多少
* synchronized和reetrantlock锁
* AQS同步器框架，countdowmlatch，cyclebarrier，semaphore，读写锁

#### 阿里二面题目：
* B-Tree索引，myisam和innodb中索引的区别
* BIO和NIO的应用场景
* 讲讲threadlocal
* 数据库隔离级别，每层级别分别用什么方法实现，三级封锁协议,共享锁排它锁，mvcc多版本并发控制协议，间隙锁
* 数据库索引？B+树？为什么要建索引？什么样的字段需要建索引，建索引的时候一般考虑什么？索引会不会使插入、删除作效率变低，怎么解决？
* 数据库表怎么设计的？数据库范式？设计的过程中需要注意什么？
* 共享锁与非共享锁、一个事务锁住了一条数据，另一个事务能查吗？
* Spring bean的生命周期？默认创建的模式是什么？不想单例怎么办？

#### 阿里三面题：
* 高并发时怎么限流
* 线程池的拒接任务策略
* HashMap和Hashtable的区别
* 实现一个保证迭代顺序的HashMap
* 说一说排序算法，稳定性，复杂度
* 说一说GC
* JVM如何加载一个类的过程，双亲委派模型中有哪些方法？
* TCP如何保证可靠传输？三次握手过程？
* springboot的启动流程
* 集群、负载均衡、分布式、数据一致性的区别与关系
* 数据库如果让你来垂直和水平拆分，谁先拆分，拆分的原则有哪些(单表数据量多大拆)
* 最后谈谈Redis、Kafka、 Dubbo，各自的设计原理和应用场景
------------------------------------------------------------------------------------------


#### 数据库
* Join（inner、left、right）的区别？
* Union和union all区别？
* ACID，具体是啥意思?
* 事务隔离级别?幻读和不可重复读的区别?
* Mysql和mongodb有啥区别?
* 计算机网路
* RPC和http的区别
* 详细描述TCP四次挥手过程
* Java中间件
* 秒杀项目会使用到哪些中间件？
* 为什么Redis是单线程？
* 如何保证Redis和数据库双写一致？
* 如何设计一个消息队列中间件？
* 分库分表后，id主键如何处理？
* 如何设计一个类似Dubbo的RPC？
* MySQL数据库中常用的搜索引擎的区别是什么？
* 索引的分类及作用？索引的工作方式是什么，为什么会让查询变得快速
* Linux操作系统下，你是如何监控服务器性能
* MySQL数据库如何监控，各指标代表了什么意思
ref:https://maimai.cn/article/detail?fid=1188457423&efid=b6VprMXHZGlLNLckx8yfAQ

------------------------------------------------------------------------------------------

#### 面试题总结  
* Java虚拟机(JVM)
* JVM内存模型结构方法区和直接内存什么时候会oom？
* JVM收集器G1的内存模型和CMS的内存模型有什么不同？
* jvm调优用过吗？
* 如何查看java内存使用情况（jconsole、命令jmap、jstack等等）
* Java集合类
* Arraylist、linkedlist差异，应用场景；
* HashMap在JDK1.8有哪些改动？
* HashCurrentMap和HashMap的区别在哪里？
* Hashmap什么时候使用红黑树？
* Java多线程
* 线程的几种状态，请画出具体的状态流转图？
* Java wait、sleep的区别？
* volatile如何实现指令重排序？
* 线程池中的阻塞队列如果满了怎么办（拒绝策略）？
* Synchronized和AQS异同，AQS公平非公平如何实现；
* 多线程里面对一个整型做加减为啥不能用volatile；
* voliatile和synchonized有什么区别？
* synchonized和jdk提供的Lock包又有什么区别？

* 算法
* 二叉树宽度遍历
* 红黑树
* 数据结构的话，链表，树，图的基本知识得懂
* 了解树的先序遍历，中序遍历，后序遍历。图的广度优先搜索算法，深度优先搜索算法。
* Spring
* Bean的生命周期；
* 什么是DI、为什么DI、DI的类型（构造器注入、方法注入）；
* Spring boot和spring的差别，tomcat如何嵌入spring boot的/spring boot中的tomcat是如何启动的；
* Spring如何解决循环依赖问题

------------------------------------------------------------------------------------------


#### 面试
* Dubbo原理，Dubbo负载均衡策略，
* 数据库分库分表，采用的技术(Mycat,Sharding-jdbc)，
* 数据库不停机数据拆分(而且业务不停，CAP限制了我，这个真不会)，
* 分布式锁及其实现原理(Mysql,ZK,Redis)，
* 表单防重提交(前端按钮置灰，后端幂等)，
* LRU，并且实现插入，删除复杂度O(1)，(面试官说用HashMap加双向链表，但是HashMap的复杂度也不是O(1)啊)，
* 微服务接口调用失败了咋整(主要根据业务类型，说了一下2PC,3PC,TCC,服务高可用,重试,熔断,降级)，
* 接口重试以及原理(Feign,Ribon,Hystric，简单说一下，原理看过，说不太上来)，
* 线程池的参数及其含义(回答了四种线程池和自定义线程池的参数，后面面试官告诉我Hystric也是这个原理)，
* 线程池参数设定依据(分CPU密集型和IO密集型)，
* Mysql索引(引擎，索引类型，事务，锁行锁表，索引底层数据结构)，
* Mysql主键自增，加入当前主键是10，删掉该条记录之后，再插入一条，主键是10还是11(这个真不会，面试官说跟引擎相关)，
* HashMap和HashTable区别，HashSet原理。面试题没有记全，每一次面试都是一次进步，加油

------------------------------------------------------------------------------------------


####  陆金所:
* 项目介绍，
* 你们这个项目为啥选用HDFS，
* 一张图片是3M(总共100张)，那会不会导致128M的block的利用率很低，
* 你们用的HBASE主要存什么数据，支持模糊查询吗，通过key获取某一条记录的原理说一下，
* Kafka消息确认模式，消费防重怎么做的，
* 如果topic新增了一个分区，你的服务如何感知到的，
* final分别修饰变量、函数、类的作用是啥，
* 说一下volatile的作用，
* HashMap的初始化长度是多少，为啥成倍扩容，线程安全，
* 如果频繁fullgc，可能是什么原因

------------------------------------------------------------------------------------------

####  好未来:
* 声明型事务和编程型事务，
* 事务的执行过程(简单说了一下往事务管理器注册事务，执行完commit或者rollback，或许面试官是想问切面和动态代理，当时没反应过来)，
* 线程安全，synchronized和Lock的原理与区别，
* 场景题，word里面输入单词，怎么错词提醒(说了一下前缀树，面试官说太占内存，于是有了下一个问题)，
* 50万个长度有10的字符串，消耗多少内存(这个不太会，大佬们求告知)，


------------------------------------------------------------------------------------------



![](https://ws4.sinaimg.cn/large/006tNc79gy1g2gxc4hcitj30ku5yse82.jpg)