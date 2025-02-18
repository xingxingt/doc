
#### 数据库的三范式是什么？
    第一范式(1NF)：强调的是列的原子性，即列不可以再分为其他几列；例如联系人表【联系人】姓名，性别，电话）,如果场景中存  
                 在家庭电话和公司电话，为了符合第一范式的要求，则需要将电话这一列拆分成(家庭电话，公司电话);在任何一个   
                 关系数据库中，第一范式是对关系模式设计的基本要求;    
    第二范式(2NF)：首先是要满足1NF，然后再包含两部分内容，一是表必须有一个主键；二是没有包含在主键中的列必须完全依赖于主键，  
                 而不能只依赖于主键的一部分  
    第三范式(3NF):在1NF基础上，任何非主属性不依赖于其它非主属性[在2NF基础上消除传递依赖]。首先是 2NF，另外非主键列必须直接依赖于主键，    
                 不能存在传递依赖。即不能存在：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键的情况;  
    ref:https://blog.csdn.net/Dream_angel_Z/article/details/45175621             

#### 一张自增表里面总共有 7 条数据，删除了最后 2 条数据，重启 mysql 数据库，又插入了一条数据，此时 id 是几？
    8

#### 如何获取当前数据库版本？
    1.select version();  
    2.mysql -V  

#### 说一下 ACID 是什么？  
    ACID表示原子性(atomicity),一致性(consistency)，隔离性(isolation)和持久性(durability)   
    原子性:一个事务必须被视为一个不可分割的最小工作单元，在一个事务中的所有操作，要么全部提交成功，要么全部失败回滚，  
          在一个事务中不存在只执行一部分操作；
    一致性：数据库总是从一个一致性的状态转换为另一个一致性的状态，例如在一个事务操作中出现了问题，原先数据的状态任然保持不变，  
           如果执行成功则数据将会转变为另一个一致性状态；  
    隔离性：通常一个事务所做的修改在最终提交之前对其他事务是不可见的；  
    持久性:事务提交后，对数据的修改、系统的影响是永久的。即使出现系统故障也能够保持。  
    
#### char 和 varchar 的区别是什么？
    char的长度是不可变的，而varchar的长度是不可变的；  
    定义一个char[10]和varchar[10]的字符串字段，存入“abcd”字符，那么char锁占用的字符长度仍然为10，不足10位则用空格填充，而  
    varchar则会立马变为长度为4，取数据的时候char则需要trim()掉空格，而varchar则不需要;   
    char的存取速度要比varchar快，因为char是定长的，方便程序的存取，但是会浪费空间，属于空间换时间效率；而varchar是的长度不固定，  
    属于以空间效率为首；  
    char的存储对英文字符(ASCII）占用1个字节，对一个汉字占用两个字节，而varchar对一个英文字符或者中文字符都占两个字节;  

#### float 和 double 的区别是什么？
    double的精度较高，有效数字是16位，而float的有效数字只有7位，  
    但是double的内存消耗是float的两倍,double的运算速度要比float慢的多；  

#### mysql的内连接、左连接、右连接有什么区别？
    内连接显示的是两个表中有关联的数据，就是求两个表的交集;  
    左链接显示左表的所有数据，右表只显示符合条件的记录，没有与左表对应的数据用null补齐；  
    右链接与左链接相反，显示右边的所有数据，左表只显示符合条件的记录,没有与右表对应的记录用null补齐;  
    ref:https://blog.csdn.net/plg17/article/details/78758593  

#### mysql索引是怎么实现的？
    mysql的索引包括Betree和hash，分别是使用B+树和散列表来实现的，对比下两种索引：  
    1,hash索引使用散列表来实现查询效率势必会高O(1),但是散列表不支持区间查询，即 where id>1 and id<10;    
    2,BeTree索引方法是使用B+树的数据结构来实现的，这种数据结构是在平衡二叉查找树的基础上做的修改，转换成n叉数类似于跳表，  
    跳表的时间复杂度为O(logn)而平衡二叉查找树的时间复杂度也为O(logn)；   
    因为索引是需要占用内存空间的如果数据量特别大的时候单张表的索引占用的内存空间就很大，所以mysql的索引存储在磁盘中，但是B+数的根节点  
    存储在内存中；为了减少磁盘IO经过改造的二叉查找树做了相关优化，例如节点分裂，节点合并；   
    索引的优点：1. 天生排序。2. 快速查找。   
    索引的缺点：1. 占用空间。2. 降低更新表的速度。  
    注意点：小表使用全表扫描更快，中大表才使用索引。超级大表索引基本无效；  
    ref:https://time.geekbang.org/column/article/77830
![](https://ws3.sinaimg.cn/large/006tNc79gy1g20qp7ogc7j311o0lw76y.jpg)

#### mysql Hash索引的弊端
1,Hash 索引仅仅能满足"=","IN"和"<=>"查询，不能使用范围查询

     由于 Hash 索引比较的是进行 Hash 运算之后的 Hash 值，所以它只能用于等值的过滤，不能用于基于范围的过滤，  
     因为经过相应的 Hash 算法处理之后的  Hash 值的大小关系，并不能保证和Hash运算前完全一样。
2,Hash 索引无法被用来避免数据的排序操作  
    
     由于 Hash 索引中存放的是经过 Hash 计算之后的 Hash 值，而且Hash值的大小关系并不一定和 Hash 运算前的键值完全一样，  
     所以数据库无法利用索引的数据来避免任何排序运算；
3,Hash 索引不能利用部分索引键查询  

    对于组合索引，Hash 索引在计算 Hash 值的时候是组合索引键合并后再一起计算 Hash 值，而不是单独计算 Hash 值，所以通过  
    组合索引的前面一个或几个索引键进行查询的时候，Hash 索引也无法被利用。

4,Hash 索引在任何时候都不能避免表扫描
    
    Hash 索引是将索引键通过 Hash 运算之后，将 Hash运算结果的 Hash 值和所对应的行指针信息存放于一个 Hash 表中，  
    由于不同索引键存在相同 Hash 值，所以即使取满足某个 Hash 键值的数据的记录条数，也无法从 Hash 索引中直接完成查询，  
    还是要通过访问表中的实际数据进行相应的比较，并得到相应的结果
    
5,Hash 索引遇到大量Hash值相等的情况后性能并不一定就会比B-Tree索引高

    对于选择性比较低的索引键，如果创建 Hash 索引，那么将会存在大量记录指针信息存于同一个 Hash 值相关联。这样要定位某  
    一条记录时就会非常麻烦，会浪费多次表数据的访问，而造成整体性能低下   

#### 怎么验证 mysql 的索引是否满足需求？
    explain SELECT * FROM agent WHERE id=3;

#### 说一下数据库的事务隔离？
    1.未提交读(read uncommitted):   
    在该事务级别，一个事务中的修改，即时未提交，对其他事务也是可见的，事务可以读取未提交的数据，也就是脏读;在实际使用中很少使用;  
    2.提交读(read committed):  
    一个事务在对数据的修改，直到提交之前对数据做的任何修改对其他事务都是不可见的，这个级别也叫做不可重复读,因为两次执行同样的查询，  
    可能会得到不一样的结果;  
    3.可重复读(repeatable read):  
    该级别事务解决了脏读的问题，该隔离级别保证了在同一事物中多次读取同样的数据结果是一致的;该隔离级别会产生幻读，幻读：当某个事务  
    在读取某个范围内的数据时，另一个事务在这个范围插入了一条数据，当之前的事务在次读取该范围内的数据时就会产生幻行;  
    4.可串行化(serializable):  
    该隔离级别是最高的事务隔离级别，它强制了事务的串行执行，避免了幻读的出现，该事务隔离级别在每次读取数据时会对读取的该行数据进行  
    加锁，所以会产生各种超时或者是锁竞争，实际中很少使用，除非要求很强的一致性;  

#### 什么是幻读  
    select 某记录是否存在，不存在，准备插入此记录，但执行 insert 时发现此记录已存在，无法插入，此时就发生了幻读。
    ref:https://segmentfault.com/a/1190000016566788  
    
#### 说一下 mysql 常用的引擎？
    常用引擎：Innodb和MyIASM   
    Innodb: 
    1,该引擎提供了对数据库ACID的事务支持，还提供了行级锁和外键的约束,它设计的目的是处理大数据容量的数据库系统；    
    2,Innodb实际上是基于mysql后台的完整系统,mysql在运行时，Innodb会建立缓冲池，用于缓存数据和索引；  
    3,该引擎不支持全文搜索,也不会存储数据的行数，当要进行count时就需要进行全表扫描; 所以当需要使用事务时该引擎是首选；  
    4,由于该引擎的锁粒度较小，所以在并发较高的情况下使用该引擎会提高效率;    
    MyIASM:
    1,该引擎是mysql的默认引擎，不支持事务，也不提供行级锁和外键的约束；  
    2,执行执行insert和update操作是会锁整个表，所以执行效率低下;  
    3,MyIASM保存了表的行数,如果执行count操作，可以直接获取行数，而不需要扫描整个表;   
    4,所以说如果数据库读多写少，不需要事务支持，该引擎是首选   
    对比:  
    1,大容量的数据集时趋向于选择Innodb。因为它支持事务处理和故障的恢复。Innodb可以利用数据日志来进行数据的恢复。
      主键的查询在Innodb也是比较快的。
    2.大批量的插入语句时（这里是INSERT语句）在MyIASM引擎中执行的比较的快，但是UPDATE语句在Innodb下执行的会比较的快，
      尤其是在并发量大的时候。

#### 说一下mysql 的行锁和表锁？
    表级锁：每次操作都会锁住整张表，开销小，加锁快，粒度大，不会出现死锁的情况，锁粒度大发生锁冲突的几率最高，并发度低；  
    行级锁：每次操作只会锁住一行，开销大，加锁慢，粒度小，会出现死锁，发生锁冲突的几率较低，并发度高；   
    如果说where条件中只用到索引项则加的是行级锁，否则是表锁； 
    共享锁：select * from tableName where ... + lock in share more  
    排他锁：select * from tableName where ... + for update   
    
#### 说一下乐观锁和悲观锁？
    悲观锁：假定会发生并发冲突，会屏蔽掉一切违反数据完整性的操作。即悲观锁每次操作数据都会认为别人会修改数据，所以每次都会上锁；    
    乐观锁：假定不会发生并发冲突，只会在提交操作时检查是否会违反数据的完整性。即认为每次操作数据时都认为别人不会修改数据，所以  
           不会加锁，只会在提交的时候检查数据是否被别人修改；适合用于读多写少的业务场景，来提高吞吐量;  
    悲观锁缺点:悲观锁无论是页锁还是行锁加锁的时间都会很长，会长时间限制其他用户无法操作数据，并发性能较低;  
    乐观锁缺点:乐观锁不能解决脏读，但是加锁的时间要比悲观锁短； 
    乐观锁的实现: 
    1,使用数据库版本记录机制实现，在表中添加version字段，每次更新数据版本号+1，提交数据的时候对比先前取出的版本号是否一致；  
    2,使用时间戳来实现，和版本号实现方法一样;  
    悲观锁的实现: 先读取数据并锁住这一行select  ... for update，然后再进行update;          
    
#### 乐观锁和悲观锁的区别  
  
    1,乐观锁的思路一般是表中增加版本字段，更新时where语句中增加版本的判断，算是一种CAS（Compare And Swep）操作，商品库存场景    
      中number起到了版本控制（相当于version）的作用（ AND number=#{number}）。  
    2,悲观锁之所以是悲观，在于他认为本次操作会发生并发冲突，所以一开始就对商品加上锁（SELECT ... FOR UPDATE），然后就可以安心  
      的做判断和更新，因为这时候不会有别人更新这条商品库存。  
    ref:https://www.jianshu.com/p/f5ff017db62a

##### mysql分库分表
ref:http://www.cnblogs.com/405845829qq/p/7552736.html

* mysql 问题排查都有哪些手段？
* 如何做 mysql 的性能优化   
ref:https://www.jianshu.com/p/4af41b682e06  

ref(含答案):https://juejin.im/post/5c8375bc5188257fdd0bd252  
https://maimai.cn/article/detail?fid=1188457423&efid=b6VprMXHZGlLNLckx8yfAQ
