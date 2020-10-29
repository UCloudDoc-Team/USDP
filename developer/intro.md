# USDP 开发指南-安装目录说明

USDP 所安装的大数据服务与组件遵循如下部署规则：

## 1. 服务安装目录

规则：

~~~shell
/srv/udp/${USDP 服务版本}/${服务名称}
~~~

以 USDP V1.0.0.0 的 HDFS 服务安装为例：

~~~shell
/srv/udp/1.0.0.0/hdfs
~~~

## 2. 服务配置文件目录

规则：

~~~shell
/etc/udp/${USDP 服务版本}/${服务名称}/${原服务配置文件相对路径}
~~~

以 USDP V1.0.0.0 的 HDFS 服务配置文件为例，其 core-site.xml 相关文件在如下目录中：

~~~shell
/etc/udp/1.0.0.0/hdfs/etc/hadoop
~~~

## 3. 服务默认的数据存储路径

在部署服务时，默认的数据存储路径可以自定义修改，但官方推荐使用默认数据存储路径，方便后期运维，本例以默认路径为例进行说明。

规则：

~~~shell
/data/udp/${USDP 服务版本}/${服务名称}
~~~

以 USDP V1.0.0.0 的 HDFS 服务的数据存储为例：

~~~shell
/data/udp/1.0.0.0/hdfs/
~~~

## 4. 服务的日志存储路径

规则：
~~~shell
/var/log/udp/${USDP 服务版本}/${服务名称}/
~~~

以 USDP V1.0.0.0 的 HBase 服务日志为例：

~~~shell
/var/log/udp/1.0.0.0/hbase/hbase-hadoop-master-x.log
~~~

当开发者需要基于某些服务、组件进行开发作业时，可以进入到相应目录进行脚本、API 等操作。如有特殊需要，也可以自行添加环境变量及classpath。
