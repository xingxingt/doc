

#### 一面
#### Java容器有哪些？哪些是同步容器,哪些是并发容器？
    容器：List,Map,Set   
    同步容器: HashTable,Vector,Collecttion.synchronized方法生成的同步容器;   
    并发容器: 
    ConcurrentHashMap：线程安全的HashMap的实现   
    CopyOnWriteArrayList：线程安全且在读操作时无锁的ArrayList   
    CopyOnWriteArraySet：基于CopyOnWriteArrayList，不添加重复元素   
    ArrayBlockingQueue：基于数组、先进先出、线程安全，可实现指定时间的阻塞读写，并且容量可以限制   
    LinkedBlockingQueue：基于链表实现，读写各用一把锁，在高并发读写操作都多的情况下，性能优于ArrayBlockingQueue   
  
#### ArrayList和LinkedList的插入和访问的时间复杂度？   
    1,ArrayList 是线性表（数组）:      
    get() 直接读取第几个下标，复杂度 O(1)   
    add(E) 添加元素，直接在后面添加，复杂度O（1）   
    add(index, E) 添加元素，在第几个元素后面插入，后面的元素需要向后移动，复杂度O（n）    
    remove（）删除元素，后面的元素需要逐个移动，复杂度O（n）   
    2,LinkedList 是双向链表的操作:     
    get() 获取第几个元素，依次遍历，复杂度O(n)    
    add(E) 添加到末尾，复杂度O(1)   
    add(index, E) 添加第几个元素后，需要先查找到第几个元素，直接指针指向操作，复杂度O(n)  
    remove（）删除元素，直接指针指向操作，复杂度O(1)   

    
#### java反射原理， 注解原理？(*回顾)
    一,java反射原理:     
    Java反射机制在程序运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个 
    对象，都能够调用它的任意一个方法和属性。这种动态的获取信息以及动态调用对象的方法的功能称为java的反射机制。    
    作用:    
    1,获取类的所有变量信息      
    2,获取类的所有方法信息(私有)   
    3,修改私有变量(重点:JVM在编译阶段会把引用常量的代码替换成具体的常量值)    
    ref: https://juejin.im/post/598ea9116fb9a03c335a99a4

    二,注解原理   
    注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。通过代理对象调用自定义注解（接口）的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。     
    https://blog.csdn.net/lylwo317/article/details/52163304    


#### 说说一致性 Hash 原理
    哈希算法是根据hash值将不同的value映射到相应的位置(节点)的过程。       
    那么在分布式中，【比如memcached】，需要将不同的缓存对象按照相应的hash算法映射到相应的机 器上去，那么当添加一台机器或者是其中某一台机器宕机之后，如果按照最原始的key%n的形式来做hash的话，需要将缓存清空，然后重新将内容映射到所有的机器上，这样的代价是巨大的,而Hash一致性算法就是用来解决这样的问题!      
    https://www.jianshu.com/p/570dc8913c20

#### 新生代分为几个区？使用什么算法进行垃圾回收？为什么使用这个算法？
    新生代分为三个区: Eden,Survivor1,Survivor2    
    使用复制算法，因为一般新生代区的对象都是朝生夕灭的，每次只有少量的对象存活下来         
    而老年代中对象的存活率一般较高，所以采用标记-整理算法     
    ref:https://mp.weixin.qq.com/s/H__cChHHyvGInQBnehmIxw?from=groupmessage&isappinstalled=0ß

#### HashMap在什么情况下会扩容，或者有哪些操作会导致扩容？
    1，向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值就会触发扩容操作    
    2，判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；   

#### HashMap push方法的执行过程？
    1,先根据key的hashcode计算出table[]的索引下标   
    2,判断table[]是否为空，如果为空则进行初始化操作(resize)   
    3,如果table[]不为空，则判断key对应的数组下标的槽位是否有元素存在，如果没有元素存在则newNode节点放入table中；    
    4,如果有元素存在在发生了Hash碰撞，先判断两个key的hashcode是否相等,如果相等再根据key的equals方法判断是否相等，如果相等则直接更新key对应的value值；  
    5,如果equals方法判断不相等则判断节点是树节点还是链表，如果是链表则直接在链表后面追加元素，如果是树节点则直接挂载在树节点上(链表和树的相互转换)      
    6,put之后判断数据量是否超过threshold，如果超过则进行resize()     

#### HashMap检测到hash冲突后，将元素插入在链表的末尾还是开头？
    末尾

#### 1.8还采用了红黑树，讲讲红黑树的特性，为什么人家一定要用红黑树而不是AVL、B树之类的？
    AVL是一种高度平衡二叉树,查找的效率很高，但是每次插入和删除都需要重新调整，所以对于一些频繁的插入和删除操作AVL树代价就比较高了;    
    而红黑树只是做到了近似平衡，并不是严格的平衡，所以在更新操作上红黑树要比AVL维护平衡成本低;   
    红黑树是一种平衡二叉查找树。它是为了解决普通二叉查找树在数据更新的过程中，复杂度退化的问题而产生的。红黑树的高度近似 log2n，所以它是近似平衡，插入、删除、查找操作的时间复杂度都是 O(logn)。      

#### https和http区别，有没有用过其他安全传输手段？  
    http缺省工作是在TCP协议的80端口,http封装的信息都是明文，未加密的    
    https缺省工作是在TCP的443端口,加入了SSL协议,需要TCP三次握手,对封装的信息进行对称加密  
    https://juejin.im/entry/58d7635e5c497d0057fae036#comment 

#### 线程池的工作原理，几个重要参数，然后给了具体几个参数分析线程池会怎么做，最后问阻塞队列的作用是什么？
    原理:    
    存放线程的对象池,池中线程的执行调度由池管理器来管理，当执行任务时从池中获取一条线程，任务执行完毕后再放回池中，这样可以避免反复创建对象带来的性能开销，节省系统资源；     
    参数:       
    阻塞队列: 线程池没有空闲的核心线程时新进入的线程被放入阻塞队列中等待;队列被放满时如果总线程数未达到设置的最大线程数时则新建非核心线程处理新任务，如果达到最大线程数则触发拒绝策略;    
    https://www.jianshu.com/p/210eab345423

#### linux怎么查看系统负载情况？
    1,在top命令下，按1，则可以展示出服务器有多少CPU，及每个CPU的使用情况   
    2,监控IO情况iostat -x 1 10命令，表示开始监控输入输出状态,rsec/s表示读入，    
    wsec/s表示每秒写入，这两个参数某一个特别高的时候就表示磁盘IO有很大压力，util表示IO使用  率，如果接近100%，说明IO满负荷运转。 

#### 请详细描述springmvc处理请求全流程？
    (1) Http请求：客户端请求提交到DispatcherServlet。    
    (2) 寻找处理器：由DispatcherServlet控制器查询一个或多个HandlerMapping，找到处理请求的Controller。    
    (3) 调用处理器：DispatcherServlet将请求提交到Controller。   
    (4)(5)调用业务处理和返回结果：Controller调用业务逻辑处理后，返回ModelAndView。   
    (6)(7)处理视图映射并返回模型： DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图。     
    (8) Http响应：视图负责将结果显示到客户端。  
    https://www.cnblogs.com/dreamworlds/p/5396112.html

#### spring 一个bean装配的过程？
    spring容器的bean是否进行懒加载   
    spring bean的生命周期从spring容器创建开始知道spring容器销毁   
    1,实例化BeanFactoryAware，BeanPostProcessor，InstantiationAwareBeanPostProcessor容器级生命周期接口    
    (调用InstantiationAwareBeanPostProcessor调用postProcessBeforeInstantiation方法)    
    2,执行bean的构造器       
    (调用InstantiationAwareBeanPostProcessor调用postProcessPropertyValues方法)    
    3,执行bean的set方法为bean注入属性    
    (BeanPostProcessor接口方法postProcessBeforeInitialization对属性进行更改！)   
    4,执行bean的init-method方法进行初始化       
    (BeanPostProcessor接口方法postProcessAfterInitialization对属性进行更改！)      
    (InstantiationAwareBeanPostProcessor调用postProcessAfterInitialization方法)        
    5,容器初始化成功,bean装配完毕     
    6,容器销毁调用bean的destory-method方法进行销毁bean     
    https://www.cnblogs.com/zrtqsk/p/3735273.html

#### spring bean的作用域？   
     1,singleton:单例模式,pring IoC 容器中只会存在一个共享的 Bean 实例，无论有多少个 Bean 引用它,
        始终指向同一对象。该模式在多线程下是不安全的;     
     2,prototype:原型模式，每次通过 Spring 容器获取 prototype 定义的 bean 时，容器都将创建 一个新的 Bean 实例，     
       每个 Bean 实例都有自己的属性和状态; 对有状态的 bean 使用 prototype 作用域，而对无状态的 bean 使用 singleton 作用域。     
     3. request:在一次 Http 请求中，容器会返回该 Bean 的同一实例。而对不同的 Http 请求则会 产生新的 Bean;   
     4,session:在一次 Http Session 中，容器会返回该 Bean 的同一实例。而对不同的 Session 请 求则会创建新的实例,
       该 bean 实例仅在当前 Session 内有效      
     5,global Session:在一个全局的 Http Session 中，容器会返回该 Bean 的同一个实例，仅在 使用 portlet context 时有效。 

#### 项目用 Spring 比较多，有没有了解 Spring 的原理？AOP 和 IOC 的原理  
     
    
    


#### 二面

#### 查询中哪些情况不会使用索引？  
    联合索引(account_name_level)      
    1,不在索引上做任何操作，例如:计算，类型转换，函数等，这会导致全表扫描     
    2,如果索引了多个列，要遵守最佳左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列     
      EXPLAIN select * from tabel index1="" and index2="" and index3=""        
    3,mysql存储引擎不能继续使用索引中范围条件（bettween、<、>、in等）右边的列   
      EXPLAIN select * from agent where  account!="zxl" and name="深圳代理1" and level="1"     
    4,索引字段上使用（！= 或者 < >）判断时，会导致索引失效而转向全表扫描    
    5,索引字段上使用 is null / is not null 判断时，会导致索引失效而转向全表扫描      
    6,索引字段使用like以通配符开头（‘%字符串’）时，会导致索引失效而转向全表扫描,但是like以通配符结束相当于   
      范围查找，索引不会失效,索引类型是range;       
      EXPLAIN select * from agent where  account like '%zxl%'    
      解决办法(使用覆盖索引和like 'zxl%'):   
      EXPLAIN select account from agent where  account like 'zxl%'        
    7,索引字段是字符串，但查询时不加单引号，会导致索引失效而转向全表扫描     
    8,索引字段使用or时,会导致索引失效而转向全表扫描          
      EXPLAIN select * from agent where  account="zxl" or name="深圳代理1" or level=1     
    9,尽量使用覆盖索引（只查询索引的列（索引列和查询列一致）），减少select *    
    https://blog.csdn.net/wuseyukui/article/details/72312574

#### 数据库索引，底层是怎样实现的，为什么要用B树索引？
    mysql的索引包括Betree和hash，分别是使用B+树和散列表来实现的，对比下两种索引：    
    1,hash索引使用散列表来实现查询效率势必会高O(1),但是散列表不支持区间查询，即 where id>1 and id<10;    
    2,BeTree索引方法是使用B+树的数据结构来实现的，这种数据结构是在平衡二叉查找树的基础上做的修改，转换成n叉数类似于跳  表，跳表的时间复杂度为O(logn)而平衡二叉查找树的时间复杂度也为O(logn)；   
    3,因为索引是需要占用内存空间的如果数据量特别大的时候单张表的索引占用的内存空间就很大，所以mysql的索引存储在磁盘中，但是B+数的根节点存储在内存中；   
    4,为了减少磁盘IO经过改造的二叉查找树做了相关优化，例如节点分裂，节点合并；   
    索引的优点：1. 天生排序。2. 快速查找。     
    索引的缺点：1. 占用空间。2. 降低更新表的速度。    
    注意点：小表使用全表扫描更快，中大表才使用索引。超级大表索引基本无效；    
    https://time.geekbang.org/column/article/77830
    
#### Mysql主从同步的实现原理？  
    在master机器上，主从同步事件会被写到特殊的log文件中(binary-log);      
    在slave机器上，slave读取主从同步事件，并根据读取的事件变化，在slave库上做相应的更改。     
    主从复制的用途:  
    1,读写分离;  2,数据备份; 3,高可用HA；4,架构扩展       
    主从复制架构形式:   
    1,一主一从;   
    2,一主多从;   
    3,多主一从(从5.7开始支持;4,双主复制(两个master);   
    5,级联复制(部分slave的数据同步不连接主节点，而是连接从节点)；    
    MySQL主从复制原理:    
    https://zhuanlan.zhihu.com/p/50597960

#### MySQL是怎么用B+树？（*回顾）
    B+树:    
    1,每个节点中子节点的个数不能超过 m，也不能小于 m/2；   
    2,根节点的子节点个数可以不超过 m/2，这是一个例外；    
    3,m 叉树只存储索引，并不真正存储数据，这个有点儿类似跳表；      
    4,通过链表将叶子节点串联在一起，这样可以方便按区间查找；      
    5,一般情况，根节点会被存储在内存中，其他节点存储在磁盘中。   
    https://time.geekbang.org/column/article/77830


#### 聚族索引和非聚族索引的区别（*回顾）    
    聚簇索引的叶子节点就是数据节点，而非聚簇索引的叶子节点仍然是索引节点，只不过有指向对应数据块的指针。       
    1,MyISAM引擎使用的是非聚族引擎,它的叶子节点中保存的实际上是指向存放数据的物理块的指针。         
    MYISAM引擎的索引文件（.MYI）和数据文件(.MYD)是相互独立的。    
    MYISAM的主键索引和二级索引没有任何区别;      
    2,InnoDB按聚簇索引+非聚族索引的形式存储数据;    
    INNODB的二级索引与主键索引有很大的不同。InnoDB的二级索引的叶子包含主键值，而不是行指针(row pointers)，    
    这减小了移动数据或者数据页面分裂时维护二级索引的开销，因为InnoDB不需要更新索引的行指针;    
    https://blog.csdn.net/alexdamiao/article/details/51934917


#### 谈谈数据库乐观锁与悲观锁?  
    https://github.com/xingxingt/doc/blob/master/interview/mysql.md

#### redis是单进程单线程的kv数据库为什么能那么快?  
    官方提供的数据是可以达到100000+的QPS（每秒内查询次数 https://redis.io/topics/benchmarks） 
    1,redis数据库是完全基于内存的操作,不会出现磁盘io；   
    2,数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的；
    3,采用单线程模式，避免了上下文的切换以及多进程或者多线程情况下的cpu切换的消耗,不需要考虑锁的问题，没有  
      加锁和释放锁的操作,以及死锁问题带来的性能消耗;   
    4,多路复用IO，非阻塞io(https://juejin.im/post/5caae9876fb9a05e1530288b)    
    5,使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM机制;     
   
#### redis为什么设计为单线程的?
    1，因为Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存的大小或者网络带宽。    
    既然单线程容易实现，而且CPU不会成为瓶颈，那就顺理成章地采用单线程的方案了;    
    2，单线程，只是在处理我们的网络请求的时候只有一个线程来处理，一个正式的Redis Server    
    运行的时候肯定是不止一个线程的(可以使用ps -T -P redis进程号查看它的子线程)   
    3，对于sunion之类的比较耗时的命令时会使redis的并发下降,单个线程对应cpu的一个core，所以可以在同一台  
    多核的服务器上启动多个redis实例组成master-master或者master-slave的形式，耗时的读命令可以完全在slave进行。    
    ref:https://blog.csdn.net/xlgen157387/article/details/79470556


* 有使用过哪些NoSQL数据库？MongoDB和Redis适用哪些场景？


* 描述分布式事务之TCC服务设计？  
  TCC是服务化的两阶段编程模型，其Try、Confirm、Cancel 3个方法均由业务编码实现；  
  第一阶段: Try操作作为一阶段，负责资源的检查和预留;    
  第二阶段: Confirm操作作为二阶段提交操作,执行真正的业务，Cancel是预留资源的取消；
  实现TCC注意事项:   
  1,业务操作如何分两阶段完成   
  2,允许空回滚(try阶段网络丢包,事务管理器发送cancel请求，tcc服务器应该能够执行cancel操作)         
  3,防悬挂控制(应当允许空回滚，但是要拒绝执行空回滚之后到来的一阶段Try请求)      
  4,幂等控制(即Try、Confirm、Cancel 执行一次和执行多次的业务结果是一样的)   
  5,业务数据可见性控制(对于中间状态的数据可见性)     
  6,业务数据并发访问控制      
  https://yq.aliyun.com/articles/609854?utm_content=m_1000005852      
  https://yq.aliyun.com/articles/609854?utm_content=m_1000005852

* Redis和memcache有什么区别？Redis为什么比memcache有优势？
* Redis 的数据结构

#### 海量数据过滤，黑名单过滤一个 url。 
    使用布隆过滤器    
    1，使用hashmap，数据量特别大的时候散列表会出现大量的hash碰撞，从而需要链表和红黑树的支持，  
    而链表有需要存储指针从而扩大了内存的存储量；  
    2，位图,某个整数是否在这1亿个整数中? 使用数下标代表整数值,下标对应的值是true/false,array[ 233 ] = True   
    空间复杂度随集合内最大元素增大而线性增大;  
    3,布隆过滤器，要判断一个值，需要通过多个计算hash值的函数，计算出多个hash值，然后判断这些hash值再数组中的值是否  
    都为1，如果为1则表示该值存在，否则不存在;   
    误判(可以建立白名单解决):     
    布隆过滤器说某个元素在，可能会被误判
    布隆过滤器说某个元素不在，那么一定不在     
    https://blog.csdn.net/dreamispossible/article/details/89972545
    https://blog.csdn.net/wypblog/article/details/8237956

#### 讲一讲AtomicInteger，为什么要用CAS而不是synchronized？  
    CAS:乐观认为并发不高，不需要阻塞，可以不上锁。   
    Synchronized: 悲观认为并发很高，需要阻塞，需要上锁。   


#### 三面

* 考虑redis的时候，有没有考虑容量？大概数据量会有多少？
* Redis 的 list zset 的底层实现
* solr和mongodb的区别，存数据为什么不用solr？
* 分布式 session 的共享方案有哪些，有什么优劣势
* 谈谈分布式锁、以及分布式全局唯一ID的实现比较？
* 集群监控的时候，重点需要关注哪些技术指标？这些指标如何优化？
* 从千万的数据到亿级的数据，会面临哪些技术挑战？你的技术解决思路？
* 数据库分库分表需要怎样来实现？
* 排序算法的复杂度，快速排序非递归实现。
* 消息中间件有哪些？他们之间的优劣势？

#### 四面

* 分布式架构设计哪方面比较熟悉
* 介绍你实践的性能优化案例，以及你的优化思路
* 介绍项目
* 谈一个你觉得你学到最多的项目，使用了什么技术，挑战在哪里
* 各种聊项目，从项目的架构设计到部署流程。
* 最近有没有学习过新技术？
* 有什么想问我的？
* 最近两年遇到的最大的挫折，从挫折中学到了什么？
* 三年到五年的职业规划？


#### HR面

* 工作中遇到的最大挑战是什么，你如何克服的？

* 你最大的优点和最大的缺点，各自说一个？

* 未来的职业发展，短期和长期的规划是什么？

* HR走流程，主要问了未来的职业规划。

