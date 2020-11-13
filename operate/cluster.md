

# 集群管理

## 创建集群资源
### 1 进入产品页面

登录UCloud云控制台后，在导航栏点击“全部产品”展开产品列表，点击 数据分析-“智能大数据平台USDP”进入产品页面。

或可将“智能大数据平台USDP”设置为快捷方式，通过控制台左侧快捷方式菜单栏点击“智能大数据平台USDP”进入，快捷方式可有效提高您后期日常的集群维护等相关操作。

### 2 点击 <kbd>【创建集群】</kbd>按钮开始创建USDP集群

### 3 设置地域和可用区信息

请根据您的需要，在创建集群向导中设置新集群所归属的地域及可用区信息。

![](/images/地域和可用区选择.png)

### 4 集群和软件设置

在此模块需要您设置并完善集群软件、集群框架的选择，包括设置新集群所需VPC和子网信息、集群名称填写、软件版本选择。

备注：集群名称也是高可用NameNode的Namespace名称，需要字母和数字的组合，首位必须是字母

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

在此处填写该集群所有节点统一的root密码。

![](/images/访问设置.png)


### 7 等待集群部署

根据集群规模不同，可能需要的部署时间会有所差异，一般情况下，集群创建大约会在30分钟左右完成。如下图所示，集群正在部署中。

当集群处于运行运行状态的时，即可点击“访问USDP”按钮，进入USDP管理页面进行集群服务规划及部署操作。

![](/images/部署中的集群.png)

## 开始部署集群服务和组件

###  使用USDP管理控制台部署服务
点击“访问USDP”按钮，进入USDP管理页面。

#### 1 进入登录页面登录到USDP管理控制台
    第一次登录需要设置admin用户名的登录密码。如下所示：
![](/images/输入登录信息.png)

###### 2 添加服务和组件
    在右上角“当前集群”点击展开，点击选择“添加服务或组件”，开始部署集群服务。
![](/images/添加服务和组件.png)

###### 3 选择服务
    您可以在“服务组合方案”出从推荐方案A\B\C中进行选择，或“自定义”您需要的服务，其中“监控”服务是默认必须选择的，您无法取消。
    
    如下图所示：
 ![](/images/选择服务.png)

###### 4 选择组件安装
    由于选项非常多，建议采用“智能推荐”的方式进行选择，或者在“智能推荐”的基础上进行修改。
    
    以下，USDP建议，请您参考：
    *  NameNode建议安装在usdp-***-masster*的节点上，否则NameNode出现异常时，主从切换会失败
    * 节点名称usdp-***-masster*的节点，建议用于部署zookeeper、Journalnode、NameNode、Resourcemanager、Hmaster、Hive等服务和组件的master节点，可视化和调度组件如Hue、oozie、kibana、zkui也建议部署在Master节点上。
    * 节点名称usdp-***-core*核心节点，建议用于存储数据（HDFS）与运行任务。建议部署datanode、nodemanager、regionserver、presto work、impala。
    * 节点名称usdp-***-task*的节点，建议用于部署计算资源，建议用来部署Nodemanager、Client。
    * 节点名称usdp-***-monitor*的节点，建议用于部署AlterManager、Grafana、InfluxDB、NodeExporter、Prometheus、USDPMonitor等监控服务。
       
    您也可根据您的需要，进行灵活规划并实时部署。

 ![](/images/选择组件安装节点.png)

  ###### 5 服务配置
     请填写所有相关服务的配置信息，其中“HIVE”、“HUE”、“KAFKA”、“KAFKAEAGLE”、“OOZIE”、“RANGER”提供了“测试连接”功能，您可以对各配置项的正确性进行测试验证。
     
     待所有的测试连接均通过后方可进入向导“下一步”。如下图所示：
 ![](/images/服务配置.png)

 ###### 6 部署信息总览
     至此，为您总览以上向导步骤中完成的所有选择及配置信息汇总展示，请确认无误后点击“开始部署”按钮开始集群的服务组件部署工作。如下图所示： 
 ![](/images/部署服务.png)

 ###### 7 安装并启动服务
    当服务安装发生错误时，您可以点击查看报错节点“失败”详情，参考详情提示信息，进行手动修复错误操作，之后点击“全部重试”按钮，重新进行服务组件部署工作。
  ![](/images/安装并部署服务.png)

  待所有节点正常安装成功后，即可点击“完成”按钮，退出集群服务安装向导。此时新集群服务已安装完成。

  其他更多操作，请参考本文档“操作指南”和“开发指南”章节的相关文档内容。
