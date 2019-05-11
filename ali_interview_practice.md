
#### TreeSet和HashSet的区别  
    1.TreeSet可以保证数据存入是有序状态的，而HashSet则是无序的；   
    2.HashSet集合元素可以为null,但是TreeSet不可为空;   
    3.HashSet是通过元素的HashCode和equals()来判断两个对象是否相同，而TreeSet则是通过equals()或者compareTo来比较是否返回0;   
    4.TreeSet底层是通过TreeMap来实现的，而TreeMap底层是基于红黑树来实现的，而HashSet实际上是通过HashMap来实现的，  
      而HashMap底层又是通过哈希表+链表+红黑树来实现的;  
    5.TreeSet的排序可以按照自然排序(升序)，或者定制排序:   
      自然排序:即使用obj1.compareTo(obj2),返回0则代表两个对象相等,返回正数则obj1>obj2，负数obj1<obj2;   
      定制排序:使用Comparator接口，实现 int compare(T o1,T o2)方法；  
      
#### HashMap如何解决冲突和扩容机制 
     HashMap使用链表+红黑树的方式来解决冲突；   
     当HashMap的容量达到阈值大小时，就会触发resize()操作，该操作会新建一个容量是原来table的2倍大小的新table，然后将旧数据表  
     的数据copy到新的table中；将旧的table表废弃掉;  

#### ConcurrentHashMap是如何做到高并发的
    concurrentHashMap在jdk1.7时使用segment分段锁技术来实现高并发,而segment继承ReetranLock来实现锁，底层使用数组+链表的形式存储;  
    而在1.8时使用CAS+synchronized来实现的高并发,底层使用数组+链表+红黑树的形式存储；    
    
#### 线程池平时怎么用   
    当需要使用多个线程去处理任务时可以使用线程池来维护线程的生命周期;    
    线程池可以解决线程的创建和销毁所产生的系统开销，解决资源不足的问题，以及大量同类线程的开启消耗内存以及过度切换的问题；      
    可以使用jdk提供的Executors工厂类来创建线程,创建线程池的核心参数:   
    corePoolSize:核心线程数，该线程即时处于空闲状态也不会被回收掉;   
    maximumPoolSize: 线程池的最大线程数,即:核心线程数+非核心线程数   
    keepAliveTime: 非核心线程如果处于空闲状态多长时间会被回收   
    TimeUnit unit: keepAliveTime的时间单位   
    workQueue:任务队列，当所有的核心线程都在工作时，新请求的线程将被放在该队列中等待，如果该队列满了,则新建非核心线程来处理;   
    handler: 线程池的拒绝策略，当线程池中的线程数量达到maximumPoolSize，并且workQueue已满时，由该策略来决定如何处置新提交的任务;   
    
    
#### 多个线程等待到某一点后统一放行的实现方式有哪些   
    1,使用cyclicbarrier可以实现阻塞等待N个线程之后统一执行，使用barrier.await()阻塞线程;     
    2,使用contDownLatch,使用atch.await();来阻塞线程，使用countDown()原子性的-1来实现；    
    cyclicbarrier可以使用reset()方法重置计数器;而CountDownLatch则不能，只能使用一次;     
    
#### 数据库索引结构
    1，betree：是使用B+树来实现的，B+树是在平衡二叉查找树的基础上做的修改，转换成了N叉树，类似于跳表的形式,时间复杂度为O(logn);       
    2，hash索引: 使用散列表的来实现的，时间复杂度为O(1)，但是散列表不支持区间查询;    
    
    
#### select * form t where a=? and b>? order by c limit 0,100如何加索引   
    使用联合索引KEY `联合索引` (`a`,`b`,`c`),可以用EXPLAIN查看索引是否生效;   
    当创建(a,b,c)联合索引时，相当于创建了(a)单列索引，(a,b)联合索引以及(a,b,c)联合索引   
    想要索引生效的话,只能使用 a和a,b和a,b,c三种组合；   
    ref:https://blog.csdn.net/Abysscarry/article/details/80792876

#### 什么是聚族索引和非聚族索引,二级索引   
    聚族索引：索引和数据存储在一块，即都在一个B-tree上，一般主键索引都叫做聚族索引;   
             mysql中的InnoDB的主键索引为聚族索引，而MyISAM存储引擎采用的是非聚族索引，即索引和数据存储是分开的;   
    非聚族索引: 索引数据和数据存储是分开的；例如:MYISAM引擎的索引文件（.MYI）和数据文件(.MYD)是相互独立的。   
    二级索引： 二级索引存储的是某条记录的主键，而不是数据存储地址，例如：InnoDB主键是聚族索引，而唯一索引，普通索引，前缀索引等都是  
             二级索引又称为辅助索引;   
    ref:https://blog.csdn.net/jijianshuai/article/details/79084874     
        https://blog.csdn.net/alexdamiao/article/details/51934917    
聚族索引InnoDB,如下图:     
聚簇索引中的每个叶子节点包含主键值、事务ID、回滚指针(rollback pointer用于事务和MVCC）和余下的列(如col2)
![](https://ws4.sinaimg.cn/large/006tNc79gy1g2linx2ca0j31ia0o20zu.jpg)  
非聚族索引MyISAM,如下图:    
MYISAM引擎的索引文件（.MYI）和数据文件(.MYD)是相互独立的。
![](https://ws3.sinaimg.cn/large/006tNc79gy1g2lipiwjqtj31ds0lcdkx.jpg)   
二级索引如下:   
![](https://ws4.sinaimg.cn/large/006tNc79gy1g2liro356bj31460l0aaw.jpg)

    
#### 解释下乐观锁和悲观锁
     乐观锁: 假定不会出现并发冲突，即乐观锁认为每次操作数据的时候都不会有其他有人修改数据，直到提交任务的时候才会去检查数据是否会被修改，  
     适合读多写少的场景来提高吞吐量;      
     悲观锁; 假定会出现并发冲突，悲观锁认为每次操作数据都会有人修改数据，所以每次都会加锁；并发性能低;     


#### 介绍下Fescar/了解CAP吗？/Redis中的CAP是怎么样的？  
     

#### 如何理解幂等？项目接口的幂等如何实现?  
     幂等性是指通过同样的参数多次调用同样的方法，返回的结果都是一样的，幂等性强调的是外界通过接口对系统内部的资源影响，例如数据库的查询操作   
     该操作是天生幂等性的，因为查询操作不会改变系统内存资源，即表数据;      
     实现方法： 可以通过全局唯一ID，去重表，插入或更新，版本控制，状态机控制,分布式锁(redis,zk等),前端也可以通过token来防止重复提交;    
     
#### 两个有序的list,求交集     
     ref:https://github.com/xingxingt/centrecode/blob/master/src/main/java/dataStructure/ListGetIntersection.java
      
     
#### JVM判断对象是否回收   
     1,引用计数法，该方法存在依赖循环的缺陷     
     2,可达性分析法(GC Root)    
       可以作为GC Root的对象: 虚拟机栈中引用的对象(栈帧中的局部变量表),方法区中静态属性引用的对象，方法区中常量引用的对象，  
       本地方法栈中Native引用的对象   
       


#### 动态代理的实现方式？Cglib和Jdk的动态代理的区别   
      可以使用jdk的proxy和CGLIB的MethondInterceptor来实现;      
      1,jdk的动态代理是使用反射机制来生成一个代理接口的匿名类，而cglib是使用asm开源包，将类的class文件加载到内存，
        通过修改其字节码生成子类来实现的；         
      2,DK动态代理只能对实现了接口的类生成代理,而不能针对类，CGLIB通过继承和引用的方式进行代理，无论目标对象有没有实现接口都可以代理，
        但是无法处理final的情况。  
        
        
#### 反射能获取类里面方法的名称吗？参数名称？参数类型?  
     可以通过Method[] ms = class.getMethods();获取类里面每个方法的名称，包括从父类中继承的方法;      
     通过Class[] paramTypes = ms[i].getParameterTypes();获取方法中参数的类型;    
     通过ms[i].getParameters()[0].getName()获取参数名称      
     ref:https://github.com/xingxingt/centrecode/blob/master/src/main/java/reflectdemo/ReflectDemo2.java      
     
#### 分布式锁有哪些主流实现方式？redis和zk的实现方法有什么区别?    
     实现方式:mysql,zk,redis;    
     mysql: 可以使用select for update来实现，优点：理解起来简单，不需要维护第三方中间件，但是需要自己加事务维护锁，性能局限于数据库;   
            比起缓存的方式实现性能较低，不适用于高并发场景;    
     zookeeper: zk是基于内部的目录的创建，删除来实现分布式锁机制，依赖：节点有序性，临时节点和事件监听；一般使用Curator对zk底层的封装     
            来实现，比较简单易行，Curator的高级API封装了分布式锁，可重入锁,读写锁机制等； 缺点：需要额外维护第三方中间件，性能和mysql   
            相差无几，增加维护成本;   
     redis:使用setNx(set if not exist)来简单实现，还可以使用Redission来实现锁机制，使用Lua脚本可以实现其原子性的操作; 
           优点：redis实现的分布式锁机制的性能比mysql和zk的性能要高，缺点是需要维护redis集群；    
     ref:http://www.dengshenyu.com/java/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F/2017/10/23/zookeeper-distributed-lock.html    
         https://juejin.im/post/5bbb0d8df265da0abd3533a5    

      
#### Threadlocal有什么作用，说下用法   
     Threadlocal的作用是实现线程间的数据隔离，获取当前线程的局部变量值，不受其他线程的影响；   
     用法：使用get获取当前ThreadLocal在当前线程中保存的变量副本；  
          使用set方法可以设置当前线程的变量副本值；   
          使用remove方法可以移除掉当前线程存放的变量值；
            
            
#### 了解CAP吗？redis中CAP是怎么样的？  
     Consistent(一致性):  对于数据存放在不同的节点上，如果某个节点上的数据更新了，在其他节点上能够读取到最新的数据则成为强一致性，
                         如果在某个节点无法读取到最新数据，则是非分布式一致性；         
     Availability(可用性):非故障的节点在合理的时间给出合理的相应，而非错误或者超时的响应,而且也不能无限的阻塞请求；          
     Partition tolerance(分区容忍性):再出现网络分区时，系统仍然能够运行；例如：集群中的某个节点网络出现了问题，但是这个集群仍然  
                         可以正常工作;    
     分布式系统往往分布在不同的机器上进行网络隔离开的，这意味着必然会出现网络断开的风险，这种场景就是 网络分区 ；而发生网络分区时  
     一致性和可用性往往两难全；   
     redis是通过主从数据的异步同步来实现数据的最终一致性；所以它并不能满足一致性要求; 但是如果客户端在redis的主节点修改了数据，然后  
     如果发生了网络分区，主节点依旧可以对外提供修改服务，所以redis满足可用性；   
                           
                           
#### 分布式事务的一致性如何保证
      1,2PC(二阶段):    
        第一阶段(投票阶段): 1,协调者向所有的参与者发送请求询问是否可以执行提交操作，并且等待所有参与者的回复；2,参与节点执行协调者询问发起为止   
        的所有事务操作，并将Undo和Redo信息写入日志;3,所有参与者回应协调者，如果参与者事务执行成功则返回“同意”消息，如果执行失败则返回"中止"消息；           第二阶段(提交执行阶段):1,如果所有的参与者在第一阶段返回的都是"同意"消息，则协调者会向所有的参与者发送"正式提交(commit)"的请求；参与者  
        正式完成事务操作，并释放资源；然后参与者向协调者发送"完成"消息，协调者接受到所有参与者的"完成"消息，完成事务；    
        2，如果在第一阶段任何一个参与者返回"中止"的消息或者协调者在超时时间内没有受到所有参与者的回复则进行事务回滚，协调者向所有参与者  
        发送“事务回滚rollback”的请求，所有的参与者接受到该请求后利用之前的Undo日志实现回滚操作并释放资源，然后回复“完成回滚”给协调者，   
        协调者接收到所有参与者的"完成回滚"消息后，取消事务;     
        
        
      
    ref:https://juejin.im/post/5aa3c7736fb9a028bb189bca#comment  
        https://juejin.im/post/5b5a0bf9f265da0f6523913b#heading-16   

   
    
