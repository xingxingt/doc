### 数据结构

#### 常用数据结构简介
    Collection:  
    List:LinkedList,ArrayList,Vector,Stack,Set,TreeSet 
    Map:HashMap,HashTable,WeakHashMap,TreeMap  
    
#### 并发集合了解哪些？
    concurrentHashMap  
    copyOnWriteArrayList  
    copyOnWriteHashSet  
    
#### 列举java的集合以及集合之间的继承关系
    例如Iterator的子类有Collection和Map，而Collection的子类有List和Set，Map的子类有HashMap和TreeMap...  
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0oh9on5hcj318a0najtt.jpg)

#### Java中的Iterator和Iterable 区别
    Ieterator是java.util包中迭代器类,而Ieterable是java.lang包中接口，好多对象都实现了Iterable接口，从而对象
    可以调用Iterator()方法;  
    ref:https://www.jianshu.com/p/b65ddde3acd8
    

#### StringBuilder与StringBuffer的区别
    StringBuffer是线程安全的，里面的方法使用`synchronized`关键字来实现；而StringBuilder是非线程安全的；   
    在单线程下，StringBuilder更高效；     
    
#### 集合类/集合框架图
![](https://ws2.sinaimg.cn/large/006tNc79gy1g5uenrsqx6j31v80lw46h.jpg)

#### 集合类以及集合框架
    1,Set:HashSet的实现是利用HashMap来做的，LinkedHashSet是继承了HashSet底层用LinkedHashMap实现的,TreeSet底层使用二叉树树实现的，
      底层基于TreeMap实现的；  
    2,List:ArrayList底层是基于数组来实现的非线程安全擅长随机访问；LinkedList底层是用链表来实现的非线程安全的擅长插入和删除操作
      ,Vector与ArrayList相似底层也是数据实现的，但是Vector是线程安全的；Stack继承Vector是一个后进先出的栈；    
    3,Map: HashMap:散列表是底层基于数组存储数据，使用数组+链表+红黑树(Hash碰撞)来实现,LinkedHashMap继承HashMap底层使用链表来存储数据的,
      LinkedHashMap既可以按照它们插入的顺序排序，也可以按它们最后一次被访问的顺序排序；TreeMap有序散列表,基于红黑树来实现；
      WeakHashMap弱引用键实现的Map,跟GC有关;  
      1,强引用：普遍对象声明的引用，存在便不会GC  
      2,软引用：有用但并非必须，发生内存溢出前，二次回收  
      3,弱引用：只能生存到下次GC之前，无论是否内存足够    
      4,虚引用：唯一目的是在这个对象被GC时能收到一个系统通知  
    ref:https://www.jianshu.com/p/63e76826e852  
        http://alexyyek.github.io/2015/04/06/Collection/   
    code:https://github.com/xingxingt/centrecode/tree/master/src/main/java/collectionandmap  

#### 容器类介绍以及之间的区别（容器类估计很多人没听这个词，Java容器主要可以划分为4个部分：List列表、Set集合、Map映射、工具类（Iterator迭代器、Enumeration枚举类、Arrays和Collections），具体的可以看看这篇博文 [Java容器类]）
    ref:http://alexyyek.github.io/2015/04/06/Collection/
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0pd0tg95tj31hc0pwjuj.jpg)    

#### List,Set,Map的区别
     Map:一个保存键值对的对象，映射的Map中不能有重复的key；
     List:List存储的单个元素容器是个有序的可以索引到元素的容器，并且里面的元素可以重复；
     Set:Set里面和List最大的区别是Set里面的元素对象不可重复。
     
#### List和Map的实现方式以及存储方式
     List:List的实现由ArrayList,LinkedList；ArrayList的存储方式是数组，特点查询快;LinkedList存储方式是链表,  
          特点是删除，插入快;           
     Map: Map的实现由HashMap,LinkedHashMap,TreeMap,weekHashMap;HashMap的存储方式是散列表,特点快速查找键值；  
          LinkedHashMap存储方式是链表；TreeMap的存储方式是对键按序存放;  
         
#### HashMap的实现原理  
    ref:http://www.importnew.com/31278.html#comment-742217  
        http://wiki.jikexueyuan.com/project/java-collection/hashmap.html
        
#### HashMap源码理解
    https://github.com/xingxingt/xingxingt.github.io/blob/master/_posts/2018-08-01-HashMap%E8%AF%A6%E8%A7%A3.md
    
#### HashMap如何put数据（从HashMap源码角度讲解）？
    1，首先计算key的hashCode并计算数组的下标位置  
    2，判断数组table是否为空，如果为空则进行初始化，调用resize()  
    3，然后判断hashCode所对应的槽位是否有数据,如果没有数据就直接new一个新的Node存入table中;  
    4,如果发生hash碰撞则先判断key的hash值是否相同，如果key的hash值相同那就再用equals判断两个key是否相同，
      如果两个key相同，那就直接覆盖掉key对应的value值;  
    5,然后继续判断是节点是否是树，如果是树则直接挂载到树节点上    
    6,如果不是树节点，那就是是链表结构，将节点添加到链表的尾部，判断链表的长度是否是大于等于8，如果是则转为红黑树；  
    7，put之后判断数据量是否超过threshold，如果超过则进行resize()  

#### HashMap怎么手写实现？
    简易版:
    ref:https://github.com/xingxingt/centrecode/blob/master/src/main/java/dataStructure/MapImplDemo.java
    
#### ConcurrentHashMap的实现原理 
    ConcurrentHashMap和HashMap一样使用table存储Node，并用key的hashcode来确定table的index下标，处理hash碰撞时也是使用  
    链表和红黑树来解决;  
    在1.7之前ConcurrentHashMap使用的是segment锁分段的技术实现的高并发，而在1.8中ConcurrentHashMap是使用CAS+synchronized
    的方法来解决并发问题；
    
    https://www.jianshu.com/p/c0642afe03e0
    1.8ref:https://juejin.im/entry/59fc786d518825297f3fa968#comment
    1.6ref:https://www.ibm.com/developerworks/cn/java/java-lo-concurrenthashmap/index.html  
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g0wfah1argj31220kudjc.jpg)        


#### ConcurrentHashMap1.8的改造优点   
     1,JDK1.8的实现降低锁的粒度，JDK1.7版本锁的粒度是基于Segment的，包含多个HashEntry，而JDK1.8锁的粒度就是HashEntry（首节点）  
     2,JDK1.8版本的数据结构变得更加简单，使得操作也更加清晰流畅，因为已经使用synchronized来进行同步，所以不需要分段锁的概念，     
       也就不需要Segment这种数据结构了，由于粒度的降低，实现的复杂度也增加了  
     3,JDK1.8使用红黑树来优化链表，基于长度很长的链表的遍历是一个很漫长的过程，而红黑树的遍历效率是很快的，代替一定阈值的链表，这样形成一个最佳拍档 
     
#### JDK1.8为什么使用内置锁synchronized来代替重入锁ReentrantLock    
     1,因为粒度降低了，在相对而言的低粒度加锁方式，synchronized并不比ReentrantLock差，在粗粒度加锁中ReentrantLock可能通过Condition来
       控制各个低粒度的边界，更加的灵活，而在低粒度中，Condition的优势就没有了  
     2,JVM的开发团队从来都没有放弃synchronized，而且基于JVM的synchronized优化空间更大，使用内嵌的关键字比使用API更加自然  
     3,在大量的数据操作下，对于JVM的内存压力，基于API的ReentrantLock会开销更多的内存，虽然不是瓶颈，但是也是一个选择依据  

#### HashTable实现原理
    HashTable又叫散列表,它继承Dictionary并实现了Map接口，它的键值都是对象，HashTable底层是用Entry[]数组+链表的结构来实现的，    
    因为HashTable是一个同步容器，所以HashTable的方法基本上都用Synchronized修饰；    
    put：先检查put的key是否为空，如果为空则返回空指针异常，否则计算key的hashcode，然后根据key的hashcode计算table的下标，如果  
    table[index]有数据存在则进行遍历，如果遇到有相同个key则进行替换，如果不相同则直接添加；  
    get:根据key的hashcode计算table的index下标，然后遍历链表，找到了返回，找不到则返回null;  


#### HashMap和HashTable的区别
     1,HashMap的key和value都允许为空,非线程安全效率要比HashTable高，不能包含重复的键，但能包含重复的值，  
       HashMap是使用数组+链表+红黑树来实现的；  
     2,HashTable的key和value不可以为空，是线程安全的，HashTable是使用数组+链表来实现的；  
     3,HashMap的迭代器(Iterator)是fial-fast(快速失败);当其他线程改变了HashMap的结构(增加或者删除)，
       则抛出ConcurrentModificationException,而HashTable是enumerator迭代器，不会出现这样的情况；
       
#### HashMap与HashSet的区别
     1,HashSet实现了Set接口，它不允许有重复的值，存储的是单对象，HaseSetHashSet使用成员对象来计算hashcode值，  
       对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false  
     2,HashMap是实现了Map接口,存储的是key-value对，而且允许有空的key或者value，不允许有重复的key但是允许  
       有重复的value,HashMap使用的是使用key来计算hashcode；其实HashSet底层是使用HashMap实现的，利用HashMap的  
       key不允许重复的特性来实现的；
       
![](https://ws3.sinaimg.cn/large/006tKfTcgy1g0wub3q225j30ym0hsjsd.jpg)

#### HashMap写数据流程
     1,首先判断table是否为空,如果为空则触发resize();   
     2,如果table不为空，则计算key的hashcode，即table中插入的索引位置i;  
     3,如果table[i]不存在，则直接将数据插入table[i]位置     
     4,如果table[i]位置已存在数据,则判断该key是否存在，如果key已存在，则直接覆盖掉table[i]位置的数据，即更新value的值；
       如果key不存在则判断table[i]对应位置是链表还是红黑树,如果是链表则遍历链表准备在链表尾部插入数据，如果链表的长度大于8  
       则将链表转为红黑树,如果该key在链表中已存在则直接覆盖掉链表中key的值；如果table[i]位置为红黑树，则将<key,value>插入  
       红黑树中；注意：红黑树中的元素个数小于6时，将红黑树转化为链表；   
     5，检查table的大小是否到达设置的阈值大小，如果到达则进行resize();  
     ref:https://www.jianshu.com/p/aa017a3ddc40  
     
![](https://ws1.sinaimg.cn/large/006tNc79gy1g2bb2qj0nlj30z10u0q6l.jpg)


#### HashMap的resize()流程
     HashMap的扩容问题：1，每次扩容为原来容量的2倍;2,如果将旧table中的数据迁移到新申请的table中，并将老的table数据释放掉;  
     步骤: 
     1,先判断table的容量是否>0,如果>0则判断table的容量是否达到上限(MAXIMUM_CAPACITY),如果达到上限则将当前的阈值(threshold)设置为  
       Integer.MAX_VALUE,然后返回旧的table;如果table的容量未达到上限则将新的table容量和阈值都设置为原来的2倍;    
       Q:这里为什么阈值达到MAXIMUM_CAPACITY就不在扩容了呢?    
       A:就算我们的内存足够大，桶数组的大小已经达到了MAXIMUM_CAPACITY，此时HashMap只是不再进行扩容了，  
         新的Key-Value对还是可以添加到HashMap中，毕竟它是通过链表和红黑树来解决Hash冲突的。
     2,如果table容量为0，但是设置了阈值，则表示初始化时设定了阈值和容量的情况，则新table的容量未旧table的阈值;  
     3,如果table的容量为0,并且未设置阈值，则新表的容量为默认容量(DEFAULT_INITIAL_CAPACITY=16),新表的阈值等于  
       默认容量16 * 默认加载因子0.75f = 12;    
     4,经过前3步骤之后，判断如果新的阈值依然为0，则根据新表容量和加载因子求出新的阈值(newCap * loadFactor);  
     5,根据计算得到新的容量和阈值构建新的table;   
     6,构建新的table后就要将旧表中的数据迁移到新的table中;即遍历旧table，然后根据位运算即(hashCode & (tableLength - 1))，将旧table    
       中的元素的hashcode映射到新表中的位置;放入新表时也要考虑到hash碰撞之后的处理，即转为链表或者红黑树; 将旧表中的元素置空  
       (oldTab[j] = null;),让JVM进行GC回收;         
     ref:https://www.jianshu.com/p/aa017a3ddc40  


#### HashSet与HashMap怎么判断元素重复？
     因为HashSet是基于HashMap来实现的，所以HashSet和HashMap都根据对象的hashCode和equals来判断的，如果对象和
     集合中元素的hashcode和equals都相同，则说明是重复元素;

* TreeMap具体实现
ref:https://juejin.im/entry/57bfab077db2a20068ebf9d2

#### 集合Set实现Hash怎么防止碰撞
     因为HashSet是基于HashMap实现的所以HashSet在添加数据的时候会经过以下判断:  
     1,先判断该元素的hashcode是否已存在，如果没有相同的hashcode则直接将该元素存入table中;  
     2,如果table中存在于该元素的hashcode相同的元素，则进一步使用equals判断两个key是否相等，如果相等说明是相同元素，  
       不存，如果hashcode相同而equals判断不相等则说明不是同一个元素，可以存；  

#### ArrayList和LinkedList的区别，以及应用场景
     1，ArrayList是基于数组实现的，而linkedList是基于双向链表实现的；  
     2，ArrayList默认初始内存为10，每次add操作都会ensureCapacity确保内存不会溢出，如果内存不够用是则进行扩容grow，  
        生成一个原来数组内存的1.5倍的新数组，然后将旧数组数据copy到新数组中
     区别点:  
     1,因为Array是基于索引(index)的数据结构，它使用索引在数组中搜索和读取数据是很快的。Array获取数据的时间复杂度是O(1),  
       但是要删除数据却是开销很大的，因为这需要重排数组中的所有数据。   
     2,相对于ArrayList，LinkedList插入是更快的。因为LinkedList不像ArrayList一样，不需要改变数组的大小，也不需要在数组  
       装满的时候要将所有的数据重新装入一个新的数组，这是ArrayList最坏的一种情况，时间复杂度是O(n)，而LinkedList中插入或  
       删除的时间复杂度仅为O(1)。ArrayList在插入数据时还需要更新索引（除了插入数组的尾部）。    
     3.类似于插入数据，删除数据时，LinkedList也优于ArrayList。     
     4.LinkedList需要更多的内存，因为ArrayList的每个索引的位置是实际的数据，而LinkedList中的每个节点中存储的是实际的数据和前后节点的位置。  
     ref:http://www.importnew.com/6629.html  

#### ArrayList的remove方法讲解

#### 数组和链表的区别  
     数组静态分配内存，链表动态分配内存；   
     数组在内存中连续，链表不连续；     
     数组元素在栈区，链表元素在堆区；   
     数组利用下标定位，时间复杂度为O(1)，链表定位元素时间复杂度O(n)； 
     数组插入或删除元素的时间复杂度O(n)，链表的时间复杂度O(1)。 
    
* 二叉树的深度优先遍历和广度优先遍历的具体实现
* 堆的结构
* 堆和树的区别
* 堆和栈在内存中的区别是什么(解答提示：可以从数据结构方面以及实际实现方面两个方面去回答)？
#### 什么是深拷贝和浅拷贝 
     浅拷贝: 浅copy会创建一个新的对象，是对原始对象的一份精确拷贝，如果原始对象中有基本数据类型，则copy的是基本数据类型的值，如果原始对象中存在  
            引用数据类型，则copy的是引用数据类型的引用地址,如果其中一个对象改变了该引用地址，则另一个对象对象也会发生改变;  
     深拷贝：深copy会copy原始对象基本数据类型值和引用对象指向的动态内存,当对象和它引用的对象一起copy时便是深copy;深copy相比浅copy花费的时间久，
            花销大;        
    ref:https://my.oschina.net/jackieyeah/blog/206391#comments    
    
#### 手写链表逆序代码
    code:https://github.com/xingxingt/centrecode/blob/master/src/main/java/dataStructure/SingleLinkedReverse.java

* 讲一下对树，B+树的理解
* 讲一下对图的理解
#### 判断单链表成环与否？
    方法一:从头遍历链表，当遍历第N个节点时，重新遍历head到N之间的节点，并拿节点N与重新遍历head~N的链表中每个节点相比较，如果有节点和N节点   
          相等，则证明有重复节点，即链表有环，否则继续向下遍历，重复以上操作; 假设链表长度为S，环点为D，则时间复杂度是0+1+2+3+….+(D+S-1)  
          = (D+S-1)*(D+S)/2 ， 可以简单地理解成 O(N*N)，无额外的存储空间，故空间复杂度为O(1);   
    方法二:从头遍历链表，每遍历一个节点将该节点的id存入HashSet数据结构中，每次遍历新节点就拿新节点与HashSet中的数据做比较，如果HashSet  
          中存有与新节点相同的元素，则表明有重复节点，即单链表有环;  HashSet每次查找数据的时间复杂度为O(1),故总的时间复杂度为1*O(D+S)  
          即O(N),空间复杂度为O(N+S-1)即O(N);   
    方法三(在时间复杂度和空间复杂度上优化): 创建两个指针(java对象的引用)p1,p2,两个指针同时指向head节点 ，然后循环遍历链表p1每次向下移动  
          一个节点个数，而p2每次向下移动两个节点个数,每次比较p1,p2所指向的节点对象是否相等，如果相等则说明链表有环；时间复杂度可以为O(n)  
          因为没有额外的存储空间，空间复杂度为O(1);  
    ref:http://blog.jobbole.com/106227/#article-comment
    
    
* 链表翻转（即：翻转一个单项链表）
* 合并多个单有序链表（假设都是递增的）
