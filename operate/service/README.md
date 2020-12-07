# 服务管理

## 在公有云端USDP集群查看“服务管理”

- 登陆[UCloud云控制台](https://console.ucloud.cn/)。
- 进入首页后，点击左上角“全部产品”，在“数据分析”类目中点击“智能大数据平台USDP”，进入USDP集群管理页面。

![node_ucloud_usdp_entrance](../../images/operate/node/node_ucloud_usdp_entrance.png)

- 在已创建的USDP集群条目右侧，点击 <kbd>集群管理</kbd> 按钮，进入USDP集群管理页面，再点击 <kbd>服务管理</kbd> 标签页面，即可查看集群已部署的服务列表信息预览。

![公有云控制台_服务管理](../../images/operate/service/公有云控制台_服务管理.png)

 <kbd>服务管理</kbd> 标签页中，仅支持该USDP创建的大数据集群已安装使用的大数据生态服务组件的“服务名称”、“版本”、服务运行“状态”等基本信息查看，如下图所示：

## 在USDP控制台查看集群“服务管理”

- 在已创建的USDP集群条目右侧，点击 <kbd>访问USDP</kbd> 按钮，进入USDP自有管理控制台。

![node_ucloud_usdp_console_entrance](../../images/operate/node/node_ucloud_usdp_console_entrance.png)

- 登陆USDP控制台

![node_usdp_console_entrance](../../images/operate/node/node_usdp_console_login.png)

在USDP控制台左侧导航栏 “服务管理”，USDP已将该集群所部署使用的所有大数据服务组件按以下6类统一归类，可点击前往查看各类中相关组件服务的管理操作方法：

- [计算类服务管理](/USDP/operate/service/compute_kind)
- [存储类服务管理](/USDP/operate/service/storage_kind)
- [监控类服务管理](/USDP/operate/service/monitor_kind)
- [可视化类服务管理](/USDP/operate/service/visual_kind)

关于USDP大数据集群中各个服务管理功能、配置修改等操作，均可参考此处完成。



对某一集群组件的配置文件管理，可参考如下方法：

- [服务配置文件管理](/USDP/operate/service/service_configer_update)