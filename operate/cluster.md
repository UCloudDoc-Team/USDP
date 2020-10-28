

## 创建集群


#### 1 进入产品页面

在“全部产品”菜单中点击“智能大数据平台USDP”进入产品页面。

也可以将“智能大数据平台USDP”设置为快捷方式，通过左侧快捷方式菜单栏点击进入。

#### 2 点击【创建集群】按钮

#### 3 设置地域和可用区信息
![](/images/地域和可用区选择.png)

#### 4 集群和软件设置

该模块提供集群软件、集群框架的选择，需要设置VPC和子网信息、集群名称、软件版本。

![](/images/集群和软件设置.png)


#### 5 节点设置

![](/images/节点设置.png)

- **Master节点**

    管理节点，建议部署服务的master节点和一些管理、可视化服务。
    除了基础服务zookeeper、Journalnode、NameNode、Resourcemanager、Hmaster、Hive的管理端部署在Master上外，一些可视化和管理组件（如Hue、Oozie、、Airflow）也建议安装于Master节点

- **core节点**

    核心节点，用于存储数据（HDFS）与运行任务。建议部署datanode、nodemanager、regionserver等服务。

- **Task节点**

     用于补充计算资源，建议只部署Nodemanager

- **Monitor节点**
    用来部署AlterManager、Grafana、InfluxDB、NodeExporter、Prometheus、USDPMonitor等监控服务

#### 6 访问设置
![](/images/访问设置.png)

填充节点root密码。

#### 7 等待集群部署

根据集群规模不同，所需要的部署时间会有所差异，创建时间基本在30分钟左右。

#### 8 使用USDP管理控制台部署服务





