# E-MapReduce助力建设企业级数据仓库

对于大部分的企业，数据一般存在两个地方，一个是业务数据库，一个是日志。一般来讲，数据库数据容量有限，对于历史标记删除的记录一般会做定时清理，但是这些数据往往还是很有价值的。数据库计算能力也有限，如果要做一些数据分析，则会浪费宝贵的计算资源。
一些数据分析会横跨不能的部门，不同的业务线，往往需要不同DB之间，甚至需要跟日志做一些关联，这时就会有一个新的部门，数据仓库部门或者数据分析部门。此部门需要做第一件事情就是需要把不同的业务线的数据统统收集到一个中心。以往选择数据处理技术往往是一些商业的数据仓库。在Hadoop技术来临之后，由于其易用性、高度扩展性、低成本的优势，受到了越来越多的公司使用。本文将简单介绍使用E-MapReduce建设数据仓库。

建立数据仓库
大致的架构如下图所示：

在RDS mysql部分的数据，可以每天晚上同步一次全量的数据到离线存储中，使用emapreduce sqoop，按照日期建立分区。
查询时，可以按照
select count(*) form cluster where ds='2016-08-28'
日志数据可以采取logservice同步到OSS中，或者使用flume同步到emapreduce hdfs中。也是按照日期做分区。
日志收集好后，就可以采取hive或者spark引擎分析日志了，比如出报表，则可以把算完的数据插入到emapreduce hbase中或者RDS mysql中，再通过 阿里云提供的quick bi出报表。 每天早上就可以看到 前一天的业务状况等信息了。

作业执行
同步作业及分析作业可以采取阿里云emapreduce提供的执行计划来运行，可以新建一个执行计划，串联多个作业，当同步作业完成后，就开始分析作业。 这里还提供了 作业失败报警，启动超时报警等实用功能。


