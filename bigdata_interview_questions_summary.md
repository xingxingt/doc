
#### spark介绍
    spark是apache的顶级开源项目,它是一个快速的通用的大数据处理引擎，它的处理方式和Hadoop的MR很相似，但是与MR不同是，Spark即基于内存，可伸缩的  
    计算框架，具有高效，低延迟的特点；  
    spark作为快速的一站式大数据处理平台，拥有各种不同的应用：批处理，流计算，机器学习，图计算，交互式查询等；  
    spark的多语言的支持:java，scala,python,R；    
    
#### spark作业提交流程   
    1,client通过submit提交作业，并执行用户代码的main函数；   
    2，然后开始启动SparkContext以及CoarseGrainedExecutorBackEnnd；  
    3，在Spark启动的时候会去初始化SparkUI，环境SparkEnv，DAGScheduler调度器划分stage，task的调度器TaskSchedulerImpl，以及调度   
       器CoarseGrainedSchedulerBackend；
    4，DAGScheduler划分完作业后将依次提交stage对应的Taskset给TaskSchedulerImpl;   
    5,TaskSchedulerImpl与Driver(DriverBackEnd)通信进行launchTask； 
    6,CoarseGrainedSchedulerBackend收到消息后用executor去启动一个task执行任务，最终是TaskRunner的run方法内运行；     
![](https://ws1.sinaimg.cn/large/006tNc79gy1g385qdbnmhj30zh0u0dkc.jpg)

#### 数据模型
    星型模型和雪花型模型;     
    星型模型: 星型模型是非正规化的结构，每个维度表都会和事实表进行关联，会出现数据冗余;   
    雪花型模型: 雪花型模型是星型模型的扩展，有一个或者多个维度表没有直接与事实表关联，通过最大限度减少数据存储量以及关联较小的表来改善查询性能，  
             去除了数据冗余；   
    ref:https://blog.csdn.net/nisjlvhudy/article/details/7889422         
    

#### 小文件的合并
    hadoop不适合处理小文件，因为小文件过多会是NameNode的元数据空间过大，因为hadoop的每个文件数据都在NameNode中维护，每块block的元数据大小   
    150字节左右，所以减少小文件可以减少NameNode元数据空间的压力;   
    1,使用hadoop的Archives解决，hadoop archives的是一种特殊的归档文件格式，一个hadoop archive对应一个目录，每个archive包含元数据  
      (_index和_masterindx)和数据文件(par_0...),元数据的_index文件记录了归档文件的文件名和文件位置信息;内部是通过MR程序将指定目录的文件  
      打包成*.har的目录文件，源文件不删除，可以手动删除;   
      缺点：创建archive文件要消耗和源文件相同的磁盘空间;     
           archive文件不支持压缩;       
           archive文件一旦生成则不能改变，如果要修改文件，则只能重新创建archive文件;    
           虽然解决了NameNode元数据空间问题，但是MR工作时还是会把多个小文件交给MR去split处理，依然是低效的;    
      ref:https://hadoop.apache.org/docs/r1.0.4/cn/hadoop_archives.html    
          https://blog.csdn.net/SunnyYoona/article/details/53870077    
    2,使用SequenceFile，相当于一种容器，可以将每个小文件的文件名为key，文件内容为value作为一条记录；这样可以将大批小文件合并成为一个
      小文件；并且支持三种压缩方式，无压缩/记录压缩/块压缩; 但是 SequenceFile 文件不能追加写入，适用于一次性写入大量小文件的操作。   
    3,Hbase,Hbase是一种K，V数据库，我们可以将小文件名作为key,文件内容作为value,进行存储;，相比使用 SequenceFile 存储小文件，   
      使用 HBase 的时候我们可以对文件进行修改，甚至能拿到所有的历史修改版本。  
      ref:https://www.iteblog.com/archives/2320.html  

#### MR与Spark的区别
    1,Spark计算的中间结果可以存储在内存中，而MR计算结果需要落盘,所以Spark比MR拥有更适合数据挖掘和机器学习需要迭代的优点；  
    2,Spark可以做低延迟的交互式查询需求；而MR先天性计算缓慢无法实现；    
    3,Spark每个作业单独调度，可以把所有作业做为一张图来进行调度，作业之间相互依赖，但在调度的过程中并行调度，速度快；  
    4,Spark提供了更加丰富的算子可供简单使用，以及更多的语言支持；  
    5,Spark拥有一站式大数据处理的功能，例如：saprkSql,流计算，机器学习，图计算等;  

* 对大数据领域有什么自己的见解
* 平常怎么学习大数据的  

#### HDFS架构
    HDFS架构：Client,NameNode，SecondNanmeNode,DataNode  
    1. HDFS集群分为两大角色：NameNode、DataNode  (Secondary Namenode)
    2. NameNode负责管理整个文件系统的元数据
    3. DataNode 负责管理用户的文件数据块
    4. 文件会按照固定的大小（blocksize）切成若干块后分布式存储在若干台datanode上
    5. 每一个文件块可以有多个副本，并存放在不同的datanode上
    6. Datanode会定期向Namenode汇报自身所保存的文件block信息，而namenode则会负责保持文件的副本数量
    7. HDFS的内部工作机制对客户端保持透明，客户端请求访问HDFS都是通过向namenode申请来进行
![](https://ws3.sinaimg.cn/large/006tNc79gy1g30010vy90j31ho0u0n45.jpg)     

#### HDFS写数据流程
    1、根namenode通信请求上传文件，namenode检查目标文件是否已存在，父目录是否存在   
    2、namenode返回是否可以上传   
    3、client请求第一个 block该传输到哪些datanode服务器上   
    4、namenode返回3个datanode服务器ABC   
    5、client请求3台dn中的一台A上传数据（本质上是一个RPC调用，建立pipeline），A收到请求会继续调用B，然后B调用C，将真个pipeline建立完成，    
       逐级返回客户端  
    6、client开始往A上传第一个block（先从磁盘读取数据放到一个本地内存缓存），以packet为单位，A收到一个packet就会传给B，B传给C；     
       A每传一个packet会放入一个应答队列等待应答   
    7、当一个block传输完成之后，client再次请求namenode上传第二个block的服务器。   
![](https://ws2.sinaimg.cn/large/006tNc79gy1g30093tqj9j30zg0mi76e.jpg)    

#### HDFS读数据流程
    1、跟namenode通信查询元数据，找到文件块所在的datanode服务器  
    2、挑选一台datanode（就近原则，然后随机）服务器，请求建立socket流   
    3、datanode开始发送数据（从磁盘里面读取数据放入流，以packet为单位来做校验）   
    4、客户端以packet为单位接收，现在本地缓存，然后写入目标文件       
![](https://ws3.sinaimg.cn/large/006tNc79gy1g300ak2ujcj313g0j2gn2.jpg)    


#### HDFS的元数据管理
     namenode对数据的管理采用了三种存储形式：   
     1,内存元数据(NameSystem)   
     2,磁盘元数据镜像文件   
     3,数据操作日志文件（可通过日志运算出元数据）   
     元数据存储机制:    
     A、内存中有一份完整的元数据(内存meta data)
     B、磁盘有一个“准完整”的元数据镜像（fsimage）文件(在namenode的工作目录中)
     C、用于衔接内存metadata和持久化元数据镜像fsimage之间的操作日志（edits文件）注：当客户端对hdfs中的文件进行新增或者修改操作，   
        操作记录首先被记入edits日志文件中，当客户端操作成功后，相应的元数据会更新到内存meta.data中
 ![](https://ws1.sinaimg.cn/large/006tNc79gy1g300lw8dxhj31380latax.jpg)       

#### NameNode的安全模式
     safemode是namenode的一种状态（active/standby/safemode安全模式）;     
     NameNode的安全模式:   
     a、namenode发现集群中的block丢失率达到一定比例时（0.01%），namenode就会进入安全模式，在安全模式下，客户端不能对任何数据进行操作，   
        只能查看元数据信息（比如ls/mkdir）
     b、如何退出安全模式？
        找到问题所在，进行修复（比如修复宕机的datanode）    
        或者可以手动强行退出安全模式（没有真正解决问题）： hdfs namenode --safemode leave    
     c、在hdfs集群正常冷启动时，namenode也会在safemode状态下维持相当长的一段时间，此时你不需要去理会，等待它自动退出安全模式即可;   
     NameNode冷启动进入安全模式的原因:   
     namenode的内存元数据中，包含文件路径、副本数、blockid，及每一个block所在datanode的信息，而fsimage中，不包含block所在的datanode信息，  
     那么，当namenode冷启动时，此时内存中的元数据只能从fsimage中加载而来，从而就没有block所在的datanode信息——>就会导致namenode认为所有   
     的block都已经丢失——>进入安全模式——>datanode启动后，会定期向namenode汇报自身所持有的blockid信息，——>随着datanode陆续启动，   
     从而陆续汇报block信息，namenode就会将内存元数据中的block所在datanode信息补全更新——>找到了所有block的位置，从而自动退出安全模式;  
 
 #### Yarn的架构
     Yarn作为通用的资源调度管理器;     
     工作流程:    
     1,客户端向ResourceManager提交应用程序，其中包括ApplicationMaster、启动ApplicationMaster的命令、用户程序等；   
     2,ResourceManager为该应用程序分配第一个Container，并与对应NodeManager通信，要求它在这个Container中启动应用程序的ApplicationMaster；  
     3,ApplicationMaster向ResourceManager注册自己，启动成功后与ResourceManager保持心跳；   
     4,ApplicationMaster向ResourceManager申请资源；   
     5,申请资源成功后，由ApplicationMaster进行初始化，然后与NodeManager通信，要求NodeManager启动Container。   
       然后ApplicationMaster与NodeManager保持心跳，从而对NodeManager上运行的任务进行监控和管理；    
     6,Container运行期间，向ApplicationMaster汇报自己的进度和状态信息，以便ApplicationMaster掌握任务运行状态，      
       从而在任务失败是可以重新启动；应用运行结束后，ApplicationMaster向ResourceManager注销自己，允许其所属的Container回收。    
![](https://ws3.sinaimg.cn/large/006tNc79gy1g300u84rzsj316u0ngjt4.jpg) 

#### Hadoop3.0做了哪些改进
* 为什么项目选择使用Spark，你觉得Spark的优点在哪里
* 了解Flink与Storm嘛，他们与Spark Streaming的区别在哪里
* 1TB文件，取重复的词，top5指定的资源的场景下，如何快速统计出来
* 
* 说说项目
* Spark哪部分用得好，如何调优
* Java哪部分了解比较好
* 聊聊并发，并发实现方法，volatile关键字说说
* HashMap的底层原理
* 为什么要重写hashcode和equals
* 说说jvm
* 各个垃圾收集器运用在什么情形
* jvm调优
* 说说io
* 为什么考虑转行呢？是因为原专业不好就业吗？





---------------------------------------------------------------------------------------------------------




# Spark

* RDD,DAG,Stage怎么理解？

* 宽依赖 窄依赖怎么理解？

* Stage是基于什么原理分割task的？

* 血统的概念

* 任务的概念

* 容错方法

* 粗粒度和细粒度

* Spark优越性

* Spark为什么快

* Transformation和action是什么？区别？举几个常用方法

* RDD怎么理解

* spark 作业提交流程是怎么样的，client和 cluster 有什么区别，各有什么作用

* spark on yarn 作业执行流程，yarn-client 和 yarn cluster 有什么区别

* spark streamning 工作流程是怎么样的，和 storm 比有什么区别

* spark sql 你使用过没有，在哪个项目里面使用的

* spark 机器学习和 spark 图计算接触过没，，能举例说明你用它做过什么吗？

* spark rdd 是怎么容错的，基本原理是什么？





# Hadoop

* https://blog.csdn.net/qq_36864672/article/details/78213304
