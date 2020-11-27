# USDP 开发指南-RANGER

RANGER 是 Hadoop 生态中的一种权限管理框架，通过其可以实现对 HDFS、Hive 等生态组件进行细粒度的权限访问控制，并且 开发者 和 集群管理员 可以通过 Ranger 组件自带的 WebUI 进行相关配置及授权管理操作，以达到集群安全性增强的目的。

在通过使用 USDP 服务创建的 Hadoop 集群中，我们通过本篇指南，以示例的形式带您了解 开发者 及 管理员 如何使用 Ranger ，了解 Ranger 的相关的一些操作方法。

本篇指南，包含如下两章内容：

* [Ranger 管理 HDFS 的访问权限](/USDP/developer/ranger/ranger_hdfs)

* [Ranger 管理 Hive 的访问权限](/USDP/developer/ranger/ranger_hive)



`注意：本篇指南是以USDP V1.0.0.0版本，涉及的集群组件的部署路径`参见[各服务部署规则](https://docs.ucloud.cn/USDP/developer/intro)。

