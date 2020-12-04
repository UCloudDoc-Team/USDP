# USDP 开发指南-Hive

Hive是Hadoop生态系统中的数据仓库产品。它可以简单方便的存储、查询和分析存储在HDFS或者HBase的数据，它将sql语句转换成MR/Tez/Spark任务，进行复杂的海量数据分析。它也提供了一系列工具，可用来多数据进行提取、转化和加载，USDP默认hive执行引擎为Tez。

## 1. Hive Cli

Hive Cli是Hive服务提供的一个方便操作Hive表的客户端。其基本操作如下：

- ### 打开Hive Cli

在USDP任一安装过hive客户端的节点，进入hadoop用户下

```shell
[hadoop@usdp-pqahfeqa-master1 ~]$ /srv/udp/1.0.0.0/hive/bin/hive

```

- ### 创建hive表

```shell
 hive (default)> create table test_hive (id int, name string);
 
 OK
 Time taken: 0.41 seconds 
```

- ### 插入数据

```shell
  hive> insert into test_hive values (1,'test_ucloud'),(2,'test_hive');
  
```

- ### 读取数据

```shell
  hive> select * from test_hive;
```

- ### 统计数据个数

```shell
  hive> select count(*) from test_hive;
```

- ### 命令行直接执行sql命令

```shell
 /srv/udp/1.0.0.0/hive/bin/hive -e "select * from test_hive"
```

## 2. Beeline

Hive提供了一个可通过JDBC方式调用的服务Hive-server2（服务端口10000）。

利用beeline客户端可以远程连接Hive-server2服务，进而对hive数据进行操作。

- ### 启动beeline客户端

```shell
[hadoop@usdp-pqahfeqa-master1 ~]$ /srv/udp/1.0.0.0/hive/bin/beeline 
```

- ### 连接hive-server2

```shell
beeline> !connect jdbc:hive2://ip:10000/default;
```

?>注解：</br> 1.  用户名、密码默认可填空值</br> 2. ip修改为hive-server2的安装机器的ip

- 数据操作

同Hive Cli

- 在命令行直接提交sql命令：

```shell
/srv/udp/1.0.0.0/hive/bin/beeline -ujdbc:hive2://ip:10000  -e "select * from test_hive"
```

