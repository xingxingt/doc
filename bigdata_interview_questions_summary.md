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

* MR与Spark的区别
* 关注哪些名人的博客
* 对大数据领域有什么自己的见解
* 平常怎么学习大数据的
* StringBuilder与StringBuffer的区别
* HashMap与Hashtable的区别
* 谈谈你对树的理解
* 数据库索引的实现
* jvm的内存模型
* jvm的垃圾收集器
* jvm的垃圾收集算法
* HDFS架构
* HDFS读写流程
* Hadoop3.0做了哪些改进
* 谈谈YARN
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
