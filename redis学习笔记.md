
#### redis基本数据结构
遵循规则:   

    create if not exists:如果不存在则创建    
    drop if no elements: 如果元素不存在，则删除该数据结构
    
1,String字符串
    
    Redis的字符串是动态字符串，是可以修改的字符串，内部是一个字符数组,内部结构类似于java的ArrayList，内部的字符串分配的实际空间capacity  
    大于字符串长度len(已占用的空间)，当字符串长度小于1MB时,扩容时加倍现有的空间，如果是大于1MB，每次只增加1MB的空间，最大长度为512MB；   
    如果存放的key-value的value是整数，则可以实现计数器功能；
 
2,list列表

    list其实是链表结构，相当于java的LinkedList； 当lpop出最后一个元素的时候，该数据结构会被自动删除，内存被回收；   
    可以使用该数据结构构件队列(右进左出),栈(右进右出)；     
    list为双向链表结构也就是说它的插入和删除的效率较高O(1),但是查询的速度很慢O(n);    
    它内部的lindex(定位,相当于get(index))，ltrim(根据start,end截取list的一个区间，区间内的都保留，区间外的都砍掉)，以及lrange这些操作  
    都要遍历链表，时间复杂度都为O(n);    
    内部使用zplist实现内存的连续性；    

3,hash字典
     
    Hash相当于java的HashMap，实现也是通过数组+链表的结构实现；不同的是rehash的不同；渐进式hash；     

4,set集合
    
    set相当于java的HashSet，无序的，唯一的，内部相当于map中的value为null，使用key的唯一性来实现的；     

5,zset有序集合   
    
    类似于java的SortSet和Hashmap的结合，所以说具有有序性，唯一性；可以对每个value赋予一个score，然后根据score来进行排序   
    内部是使用跳表来实现的；时间复杂度为O(logn)
     
#### 分布式锁
    
    使用setnx(set if not exists)来实现分布式锁，使用完锁之后del key来释放锁，但是这里会出现没有调用del指令的问题，  
    所以引入了过期时间,对key加expire，即便del指令不执行，过期时间依然可以del掉该key;    
    但是setnx和expire依然无法保证操作的原子性，依然可能会出现死锁的情况；lua脚本可以保证脚本命令的所有操作具有原子性；   
    
#### 异步消息队列
    
    redis使用list可实现异步消息队列，用rpush,lpush和lpop,rpop来实现生产和消费；支持多个消费者和生产者并发的进出队列；    
    可以使用blpop和brpop来实现阻塞的读，防止列表空的时候消费者空转；如果阻塞读一直阻塞在哪里，redis的客户端连接就会被当做空闲  
    连接，服务器会主动断开连接，来减少资源占用消耗；      
    redis做消息队列可靠性差，因为他没有作为消息队列的一些高级特性，没有ack保证等；    
    redis和MQ的对比：https://blog.csdn.net/jordandandan/article/details/68946839   
    
#### HyperLogLog
    
    HyperLogLog是redis中的高级数据结构，可以实现计数功能,例如实现UV的统计,但是不太精确，标准误差是0.81%,它提供了两个指令  
    pfadd(增加计数)和pfcount(获取计数),pfmerge可以将多个pf的计算值累加在一起形成一个新的pf；该数据结构需要占据18KB的内存空间   
    所以不适合做单个用户的统计信息；  
    内部使用稀疏矩阵->稠密矩阵来实现;    
    用于统计精确度要求不高的统计问题！   
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
