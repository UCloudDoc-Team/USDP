# USDP 开发指南-Hbase


HBase是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，它支持通过key/value存储来支持实时分析，也支持通过map-reduce支持批处理分析。

## 1. HBase shell

Hbase shell是简单的，通过shell与HBase交互的方式，以下介绍使用方法：

#### 1.1 启动shell

```
[root@usdp-******-master1 ~]# /srv/udp/1.0.0.0/hbase/bin/hbase shell
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
Version 1.4.10, rUnknown, Mon Jun 29 17:57:22 CST 2020
```

#### 1.2 创建表格，并插入3条数据

```
hbase(main):004:0> create 'test_ucloud', 'cf'
0 row(s) in 1.2280 seconds

=> Hbase::Table - test_ucloud
hbase(main):005:0> list 'test_ucloud'
TABLE
test_ucloud
1 row(s) in 0.0180 seconds

=> ["test_ucloud"]
hbase(main):006:0> put 'test_ucloud', 'row1', 'cf:a', 'value1'
0 row(s) in 0.1780 seconds

hbase(main):007:0> put 'test_ucloud', 'row2', 'cf:b', 'value2'
0 row(s) in 0.0060 seconds

hbase(main):008:0> put 'test_ucloud', 'row3', 'cf:c', 'value3'
0 row(s) in 0.0060 seconds
```

#### 1.3 读取全部数据

```
hbase(main):009:0> scan 'test_ucloud'
ROW                                            COLUMN+CELL
 row1                                          column=cf:a, timestamp=1480993562401, value=value1
 row2                                          column=cf:b, timestamp=1480993575088, value=value2
 row3                                          column=cf:c, timestamp=1480993587152, value=value3
3 row(s) in 0.0610 seconds
```

#### 1.4 get一行数据

```
hbase(main):010:0> get 'test_ucloud', 'row1'
COLUMN                                         CELL
 cf:a                                          timestamp=1480993562401, value=value1
1 row(s) in 0.0090 seconds
```

#### 1.5 删除表

```
hbase(main):011:0> disable 'test_ucloud'
0 row(s) in 2.3550 seconds

hbase(main):012:0> drop 'test_ucloud'
0 row(s) in 1.4980 seconds
```

#### 1.6 退出HBase shell

```
hbase(main):013:0> exit
```


## 2. HBase应用开发

### 2.1 使用JAVA读取HBase（实现创建表格、插入数据，展示数据操作）

此示例需要您先登陆USDP集群任意安装有HBase的节点，以下操作默认在master1节点执行

#### 2.1.1 构建JAVA代码

```
mkdir  -p /data/hbase-example
cd /data/hbase-example
touch HbaseJob.java
```

HbaseJob.java代码如下

``` java
import java.util.ArrayList;
import java.util.List;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.KeyValue;
import org.apache.hadoop.hbase.client.Delete;
import org.apache.hadoop.hbase.client.Get;
import org.apache.hadoop.hbase.client.HBaseAdmin;
import org.apache.hadoop.hbase.client.HTable;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.util.Bytes;

public class HbaseJob {
    static Configuration conf=null;
    static{
        conf=HBaseConfiguration.create();//hbase的配置信息
    }
    public static void main(String[] args)throws Exception {
        HbaseJob t=new HbaseJob();
        t.createTable("person", new String[]{"name","age"});
        t.insertRow("person", "1", "age", "hehe", "100");
        t.insertRow("person", "2", "age", "haha", "101");
        t.showAll("person");
    }

    /***
     * 创建一张表
     * 并指定列簇
     * */
    public void createTable(String tableName,String cols[])throws Exception{
        HBaseAdmin admin=new HBaseAdmin(conf);//客户端管理工具类
        if(admin.tableExists(tableName)){
            System.out.println("此表已经存在.......");
        }else{
            HTableDescriptor table=new HTableDescriptor(tableName);
            for(String c:cols){
                HColumnDescriptor col=new HColumnDescriptor(c);//列簇名
                table.addFamily(col);//添加到此表中
            }
            admin.createTable(table);//创建一个表
            admin.close();
            System.out.println("创建表成功!");
        }
    }

    public  void insertRow(String tableName, String row,
            String columnFamily, String column, String value) throws Exception {
        HTable table = new HTable(conf, tableName);
        Put put = new Put(Bytes.toBytes(row));
        put.add(Bytes.toBytes(columnFamily), Bytes.toBytes(column),
                Bytes.toBytes(value));
        table.put(put);
        table.close();//关闭
        System.out.println("插入一条数据成功!");
    }

    public void showAll(String tableName)throws Exception{
        HTable h=new HTable(conf, tableName);
        Scan scan=new Scan();
        ResultScanner scanner=h.getScanner(scan);
        for(Result r:scanner){
            System.out.println("====");
            for(KeyValue k:r.raw()){
                System.out.println("行号:  "+Bytes.toStringBinary(k.getRow()));
                System.out.println("时间戳:  "+k.getTimestamp());
                System.out.println("列簇:  "+Bytes.toStringBinary(k.getFamily()));
                System.out.println("列:  "+Bytes.toStringBinary(k.getQualifier()));
                String ss=  Bytes.toString(k.getValue());
                System.out.println("值:  "+ss);
            }
        }
        h.close();
    }
}
```

#### 2.1.2 构建编译程序

- 创建编译目录和文件

```
cd /data/hbase-example
touch hbase-test.sh
```

- hbase-test.sh 代码如下：

```
#!/bin/bash
HBASE_HOME=/home/hadoop/hbase
CLASSPATH=.:$HBASE_HOME/conf/hbase-site.xml

for i in ${HBASE_HOME}/lib/*.jar ;
do
      CLASSPATH=$CLASSPATH:$i
done
#编译程序
javac -cp $CLASSPATH HbaseJob.java
#执行程序
java -cp $CLASSPATH HbaseJob
```

#### 2.1.3 执行HBase程序

编译程序目录下执行

```
sh hbase-test.sh
```

执行结果如下：

```
创建表成功!
插入一条数据成功!
插入一条数据成功!
====
行号:  1
时间戳:  1480991139173
列簇:  age
列:  hehe
值:  100
====
行号:  2
时间戳:  1480991139240
列簇:  age
列:  haha
值:  101
```

## 3. HBase日常运维操作

以下操作需要在USDP集群master节点下以hadoop用户下执行，否则会出现权限不足提示

- 查看hbase region状态信息查询

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck
```

- 查看常见的数据不一致修复方法 

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck --help

Unrecognized option:--help
Usage: fsck [opts] {only tables}
 where [opts] are:
   -help Display help options (this)
   -details Display full report of all regions.
   -timelag <timeInSeconds>  Process only regions that  have not experienced any metadata updates in the last  <timeInSeconds> seconds.
   -sleepBeforeRerun <timeInSeconds> Sleep this many seconds before checking if the fix worked if run with -fix
   -summary Print only summary of the tables and status.
   -metaonly Only check the state of the hbase:meta table.
   -sidelineDir <hdfs://> HDFS path to backup existing meta.
   -boundaries Verify that regions boundaries are the same between META and store files.
   -exclusive Abort if another hbck is exclusive or fixing.

  Metadata Repair options: (expert features, use with caution!)
   -fix              Try to fix region assignments.  This is for backwards compatiblity
   -fixAssignments   Try to fix region assignments.  Replaces the old -fix
   -fixMeta          Try to fix meta problems.  This assumes HDFS region info is good.
   -noHdfsChecking   Don't load/check region info from HDFS. Assumes hbase:meta region info is good. Won't check/fix any HDFS issue, e.g. hole, orphan, or overlap
   -fixHdfsHoles     Try to fix region holes in hdfs.
   -fixHdfsOrphans   Try to fix region dirs with no .regioninfo file in hdfs
   -fixTableOrphans  Try to fix table dirs with no .tableinfo file in hdfs (online mode only)
   -fixHdfsOverlaps  Try to fix region overlaps in hdfs.
   -fixVersionFile   Try to fix missing hbase.version file in hdfs.
   -maxMerge <n>     When fixing region overlaps, allow at most <n> regions to merge. (n=5 by default)
   -sidelineBigOverlaps  When fixing region overlaps, allow to sideline big overlaps
   -maxOverlapsToSideline <n>  When fixing region overlaps, allow at most <n> regions to sideline per group. (n=2 by default)
   -fixSplitParents  Try to force offline split parents to be online.
   -removeParents    Try to offline and sideline lingering parents and keep daughter regions.
   -ignorePreCheckPermission  ignore filesystem permission pre-check
   -fixReferenceFiles  Try to offline lingering reference store files
   -fixHFileLinks  Try to offline lingering HFileLinks
   -fixEmptyMetaCells  Try to fix hbase:meta entries not referencing any region (empty REGIONINFO_QUALIFIER rows)

  Datafile Repair options: (expert features, use with caution!)
   -checkCorruptHFiles     Check all Hfiles by opening them to make sure they are valid
   -sidelineCorruptHFiles  Quarantine corrupted HFiles.  implies -checkCorruptHFiles

  Metadata Repair shortcuts
   -repair           Shortcut for -fixAssignments -fixMeta -fixHdfsHoles -fixHdfsOrphans -fixHdfsOverlaps -fixVersionFile -sidelineBigOverlaps -fixReferenceFiles -fixHFileLinks -fixTableLocks -fixOrphanedTableZnodes
   -repairHoles      Shortcut for -fixAssignments -fixMeta -fixHdfsHoles

  Table lock options
   -fixTableLocks    Deletes table locks held for a long time (hbase.table.lock.expire.ms, 10min by default)

  Table Znode options
   -fixOrphanedTableZnodes    Set table state in ZNode to disabled if table does not exists

 Replication options
   -fixReplication   Deletes replication queues for removed peers

```

- 存在inconsistencies region修复 

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck -repair
```

- 修复hbase空洞 

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck -fixHdfsHoles
```

- 修复meta信息（根据meta表，将表上的region分给regionserver）

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck -fixMeta
```

- 重新修复meta表（根据hdfs上的regioninfo文件，生成meta表）

```
/srv/udp/1.0.0.0/hbase/bin/hbase hbck -fixAssignments
```

- 开启region自动均衡

需要在hbase shell下开启：

```
[root@usdp-******-master1 ~]# /srv/udp/1.0.0.0/hbase/bin/hbase shell
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
Version 1.4.10, rUnknown, Mon Jun 29 17:57:22 CST 2020

hbase(main):001:0> balance_switch true
```

