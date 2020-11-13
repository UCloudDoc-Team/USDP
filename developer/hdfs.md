# USDP 开发指南-HDFS

HDFS 是一个具有高容错、高吞吐特性的分布式文件系统。HDFS 的设计架构易于扩展也易于使用，适合存储海量文件。

``注：下面以 USDP V1.0.0.0 版本为例进行说明。``

## 1. HFDS 基础操作

- ### 查询文件


~~~shell
/srv/udp/1.0.0.0/hdfs/bin/hdfs dfs -ls [-d] [-h] [-R] [<path>]
或
/srv/udp/1.0.0.0/hdfs/bin/hadoop fs -ls [-d] [-h] [-R] [<path>]
~~~

- ### 上传文件


~~~shell
/srv/udp/1.0.0.0/hdfs/bin/hdfs dfs -put [-f] [-p] [-l]
或
/srv/udp/1.0.0.0/hdfs/bin/hadoop fs -put [-f] [-p] [-l]
~~~

- ### 下载文件


~~~shell
/srv/udp/1.0.0.0/hdfs/bin/hdfs dfs -get [-p] [-ignoreCrc] [-crc]
或
/srv/udp/1.0.0.0/hdfs/bin/hadoop fs -get [-p] [-ignoreCrc] [-crc]
~~~

``注：更多请参考 hadoop fs -help``

## 2. WebHDFS 接口

WebHDFS 提供 HDFS 的 RESTful 接口，可通过此接口进行 HDFS 文件操作。使用 WebHDFS 时，客户端是先通过 Namenode 节点获取文件所在的 Datanode 地址，再通过与 Datanode 节点进行数据交互。

- ### 上传文件


USDP 集群默认开启 HDFS NameNode 组件的高可用，同一时刻，只有一个节点处于 [active] 状态，另外一个 NameNode 组件处于 [standby] 状态。

``注：在进行 WebHDFS 接口操作时，请先确保所调用接口的 NameNode 处于 [active] 状态。``

​        -    准备数据

~~~shell
touch ucloud.txt
echo "ucloud" > ucloud.txt
~~~

* 创建文件请求

  ~~~shell
  curl -i -X PUT "http://<ActiveNameNode Hostname>:50070/webhdfs/v1/tmp/ucloud.txt?op=CREATE"
  ~~~

  通过该请求可获得文件存储的 DataNode 地址：

  ~~~shell
  HTTP/1.1 307 TEMPORARY_REDIRECT
  Location: http://<DATANODE>:<PORT>/webhdfs/v1/<PATH>?op=CREATE...
  Content-Length: 0
  ~~~

  进而可以发起上传文件请求。

* 上传文件请求

  ~~~shell
  curl -i -X PUT -T usdp.txt "http://<DataNode Hostname>:50075/webhdfs/v1/tmp/ucloud.txt?op=CREATE&namenoderpcaddress=<ClusterName>&overwrite=false"
  ~~~

### 2.2 追加文件

* 准备数据

  ~~~shell
  touch append_ucloud.txt
  echo "ucloud" > append_ucloud.txt
  ~~~

* 获取 HDFS 上被追加的文件地址

  ~~~shell
  curl -i -X POST "http://<ActiveNameNode Hostname>:50070/webhdfs/v1/tmp/ucloud.txt?op=APPEND"
  ~~~

  通过该请求可获得文件存储的 DataNode 地址：

  ~~~shell
  HTTP/1.1 307 TEMPORARY_REDIRECT
  Location: http://<DATANODE>:<PORT>/webhdfs/v1/<PATH>?op=CREATE...
  Content-Length: 0
  ~~~

  进而可以发起追加文件请求。

* 追加文件

  ~~~shell
  curl -i -X POST -T append_ucloud.txt "http://<DataNode Hostname>:50075/webhdfs/v1/tmp/ucloud.txt?op=APPEND&namenoderpcaddress=<ClusterName>"
  ~~~

### 2.3  读取文件

~~~shell
curl -i -L "http://<ActiveNameNode Hostname>:50070/webhdfs/v1/tmp/ucloud.txt?op=OPEN"
~~~

### 2.4 删除文件

~~~shell
curl -i -X DELETE "http://<ActiveNameNode Hostname>:50070/webhdfs/v1/tmp/ucloud.txt?op=DELETE"
~~~

## 3.  HttpFS 接口

HttpFS 与 WebHDFS 的区别是：HttpFS 不需要客户端访问集群的每一个节点，只需授权访问启动了 HttpFS 服务的单台机器即可。由于 HttpFS 是在内嵌的 Tomcat 中一个 Web 应用，因此性能上会受到一些限制。

### 3.1 上传文件

* 准备文件

  ~~~shell
  touch httpfs_ucloud.txt
  echo "httpfs_ucloud" > httpfs_ucloud.txt
  ~~~

* 上传文件

  ~~~shell
  curl -i -X PUT -T httpfs_ucloud.txt --header "Content-Type: application/octet-stream" "http://<HttpFS Hostname>:14000/webhdfs/v1/tmp/httpfs_ucloud.txt?op=CREATE&user.name=hadoop&data=true"
  ~~~

  ``注：url中需添加 user.name，否则会报"HTTP Status 401 - Authentication required"错误``

### 3.2  追加文件

* 准备文件

  ~~~shell
  touch append_ucloud.txt
  echo "append_ucloud" > append_ucloud.txt
  ~~~

* 追加文件

  ~~~shell
  curl -i -X POST -T append_ucloud.txt --header "Content-Type: application/octet-stream" "http://<HttpFS Hostname>:14000/webhdfs/v1/tmp/httpfs_ucloud.txt?op=APPEND&user.name=hadoop&data=true"
  ~~~

### 3.3  读取文件

~~~shell
curl -i -L "http://<HttpFS Hostname>:14000/webhdfs/v1/tmp/httpfs_ucloud.txt?op=OPEN&user.name=hadoop"
~~~

### 3.4  删除文件

~~~shell
curl -i -X DELETE "http://<HttpFS Hostname>:14000/webhdfs/v1/tmp/httpfs_ucloud.txt?op=DELETE&user.name=hadoop"
~~~

## 4. HDFS 日常运维

### 4.1  组件启停

USDP 控制台页面可以对服务下的组件进行启停操作，除此之外，用户也可以通过 SSH 登录到节点执行脚本对组件进行启停操作。

``注：下面将以 USDP V1.0.0.0 为例进行说明。``

* NameNode 启停

  ~~~shell
  /srv/udp/1.0.0.0/hdfs/sbin/start-namenode.sh
  /srv/udp/1.0.0.0/hdfs/sbin/stop-namenode.sh
  ~~~

* DataNode 启停

  ~~~shell
  /srv/udp/1.0.0.0/hdfs/sbin/start-datanode.sh
  /srv/udp/1.0.0.0/hdfs/sbin/stop-datanode.sh
  ~~~

* ZKFC 启停

  ~~~shell
  /srv/udp/1.0.0.0/hdfs/sbin/start-zkfc.sh
  /srv/udp/1.0.0.0/hdfs/sbin/stop-zkfc.sh
  ~~~

* JournalNode 启停

  ~~~shell
  /srv/udp/1.0.0.0/hdfs/sbin/start-journalnode.sh
  /srv/udp/1.0.0.0/hdfs/sbin/stop-journalnode.sh
  ~~~

* HttpFS 启停

  ~~~shell
  /srv/udp/1.0.0.0/hdfs/sbin/start-httpfs.sh
  /srv/udp/1.0.0.0/hdfs/sbin/stop-httpfs.sh
  ~~~

### 4.2  查看 HDFS 状态

~~~shell
[hadoop] $ /srv/udp/1.0.0.0/hdfs/bin/hdfs dfsadmin -report
~~~

### 4.3 修改 HDFS 文件副本数量

~~~shell
[hadoop] $ /srv/udp/1.0.0.0/hdfs/bin/hdfs dfs -setrep -R [replication-factor] [targetDir]
~~~

例如：修改HDFS 根目录下文件副本数量为 2。

~~~shell
[hadoop] $ /srv/udp/1.0.0.0/hdfs/bin/hdfs dfs -setrep -R 2 /
~~~

### 4.4 查看 HDFS 文件系统状态

~~~shell
[hadoop] $ /srv/udp/1.0.0.0/hdfs/bin/hadoop fsck /
~~~

返回结果示例如下：

~~~shell
Connecting to namenode via http://udp02:50070/fsck?ugi=hadoop&path=%2F
FSCK started by hadoop (auth:SIMPLE) from /10.23.38.64 for path / at Thu Oct 29 13:13:00 CST 2020

Status: HEALTHY
 Total size:    1103087995 B (Total open files size: 827 B)
 Total dirs:    94
 Total files:   1000
 Total symlinks:                0 (Files currently being written: 8)
 Total blocks (validated):      998 (avg. block size 1105298 B) (Total open file blocks (not validated): 7)
 Minimally replicated blocks:   998 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          5
 Number of racks:               1
FSCK ended at Thu Oct 29 13:13:00 CST 2020 in 59 milliseconds


The filesystem under path '/' is HEALTHY
~~~

``注：述HEALTHY表示当前HDFS文件系统正常，无坏块或者数据丢失``
