
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

#### 什么是聚族索引和非聚族索引   
    
    
    
#### 解释下乐观锁和悲观锁
     乐观锁: 假定不会出现并发冲突，即乐观锁认为每次操作数据的时候都不会有其他有人修改数据，直到提交任务的时候才会去检查数据是否会被修改，  
     适合读多写少的场景来提高吞吐量;      
     悲观锁; 假定会出现并发冲突，悲观锁认为每次操作数据都会有人修改数据，所以每次都会加锁；  


    
    
    
