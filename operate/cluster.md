

# 集群管理

## 创建集群
### 1 进入产品页面

在“全部产品”菜单中点击“智能大数据平台USDP”进入产品页面。

也可以将“智能大数据平台USDP”设置为快捷方式，通过左侧快捷方式菜单栏点击进入。

### 2 点击【创建集群】按钮

### 3 设置地域和可用区信息
![](/images/地域和可用区选择.png)

### 4 集群和软件设置

该模块提供集群软件、集群框架的选择，需要设置VPC和子网信息、集群名称、软件版本。

![](/images/集群和软件设置.png)


###  5 节点设置

![](/images/节点设置.png)

- **Master节点**

    管理节点，主要用来部署zookeeper、Journalnode、NameNode、Resourcemanager、Hmaster、Hive等管理服务，可视化组件如Hue、oozie也建议部署在Master节点上。

- **core节点**

    核心节点，用于存储数据（HDFS）与运行任务。建议部署datanode、nodemanager、regionserver等服务。

- **Task节点**

     用于补充计算资源，建议只部署Nodemanager

- **Monitor节点**

    用来部署AlterManager、Grafana、InfluxDB、NodeExporter、Prometheus、USDPMonitor等监控服务

### 6 访问设置
![](/images/访问设置.png)

填充节点root密码。

### 7 等待集群部署

根据集群规模不同，所需要的部署时间会有所差异，创建时间基本在30分钟左右。如下图所示，集群正在部署中。

当集群处于运行运行状态的时候，可以点击访问USDP进入USDP管理页面，进行服务部署操作。

![](/images/部署中的集群.png)

## 部署服务和组件

###  使用USDP管理控制台部署服务
点击访问USDP进入USDP管理页面

#### 1 进入登录页面登录到USDP管理控制台
    输入用户名和密码登录USDP
![](/images/输入登录信息.png)

###### 2 添加服务和组件
    在右上角选择添加服务和组件，开始部署服务。
![](/images/添加服务和组件.png)

###### 3 选择服务
    您可以在“服务组合方案”从推荐方案A\B\C中进行选择，或“自定义”您需要的服务，其中“监控”服务是默认必须选择的，无法取消


    如下图所示：
 ![](/images/选择服务.png)

###### 4 选择组件安装
    由于选项非常多，建议采用“智能推荐”的方式进行选择，或者在“智能推荐”的基础上进行修改。
  
    节点名称usdp-***-masster*的节点建议用来部署zookeeper、Journalnode、NameNode、Resourcemanager、Hmaster、Hive等服务和组件的master节点，可视化和调度组件如Hue、oozie、kibana、zkui也建议部署在Master节点上。

    节点名称usdp-***-core*核心节点，用于存储数据（HDFS）与运行任务。建议部署datanode、nodemanager、regionserver、presto work、impala。

    节点名称usdp-***-task*的节点用于部署计算资源，建议用来部署Nodemanager、Client
    
    节点名称usdp-***-monitor*的节点建议用来部署AlterManager、Grafana、InfluxDB、NodeExporter、Prometheus、USDPMonitor等监控服务
    
 ![](/images/选择组件安装节点.png)
 
  ###### 5 服务配置
     请填写所有相关服务的配置信息
     其中“HIVE”、“HUE”、“KAFKA”、“KAFKAEAGLE”、“OOZIE”、“RANGER”提供了“测试连接”功能，您可以对配置的正确性进行验证，所有的测试连接通过后才能进入向导“下一步”。如下图所示：
 ![](/images/服务配置.png)
 
 ###### 6 部署信息总览
     到此，为您总览以上向导步骤中完成的所有选择及配置信息汇总展示，确认无误后点击“开始部署”按钮开始集群部署工作。如下图所示： 
 ![](/images/部署服务.png)

 ###### 7 安装并启动服务
    当服务安装发生错误时，可以点击查看节点“失败”详情，参考提示进行手动修复错误后，点击“全部重试”按钮重新进行安装，
  ![](/images/安装并部署服务.png)
  
  待所有节点正常安装成功后，可以点击“完成”按钮，退出集群服务安装向导。此时新集群服务已安装完成。
  
  服务部署之后，更多操作请参考服务操作和开发指南部分
