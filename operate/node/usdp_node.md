# USDP控制台集群节点管理

通过本章节介绍，即可在USDP控制台中对当前集群的所有节点管理操作，如各节点上组件部署情况查询和管理、节点资源监控（CPU、内存、磁盘等维度）、节点资源监控图表、节点运行查看等。

- [集群各节点运行的组件查看](/USDP/operate/node/usdp_node?id=集群各节点运行的组件查看)
- [集群单节点管理](/USDP/operate/node/usdp_node?id=集群单节点管理)
- [集群单节点上的组件管理](/USDP/operate/node/usdp_node?id=集群单节点上的组件管理)

## 集群各节点运行的组件查看

在当前集群的所有节点列表信息页面，已针对单个节点上部署的大数据服务组件进行统计，如下图所示：

![usdp_console_node_ component_count](../../images/operate/node/usdp_node/usdp_console_node_component_count.png)

点击统计数字，即可在弹出的对话框中查看到该接节点上所有已部署的组件及组件运行状态，方便集中的、滚动查看。如下图所示：

![usdp_console_node_ component_show](../../images/operate/node/usdp_node/usdp_console_node_component_show.png)

## 集群单节点管理

在当前集群的所有节点列表信息页面，点击“节点域名”中单个节点域名快捷方式，进入该节点的详情概览页面。如下图所示：

![usdp_console_node_single_entrance](../../images/operate/node/usdp_node/usdp_console_node_single_entrance.png)

在节点“概览”页面，已针对当前节点的系统监控指标实现图表展示，如下图所示：

![usdp_console_node_single_details](../../images/operate/node/usdp_node/usdp_console_node_single_details.png)

?>概览页支持按既定的时间范围查看当前节点的性能表现，支持自定时间范围。</br>可选的有：</br>- **1小时内**</br>- **6小时内**</br>- **12小时内**</br>- **1天内**</br>- **7天内**</br>- **15天内**</br>- **30天内**</br>- **自定义**

对当前节点更加丰富的监控指标维度图表信息查看，可参考公有云控制台USDP集群节点监控信息页面查看。

## 集群单节点上的组件管理

点击切换至 <kbd>组件管理</kbd> 标签页，USDP已将该节点上所以已部署的组件列表展示出来，为便于查找组件，该页面支持按“组件名称”、“所属服务”进行搜索。在当前页面中，可查看已部署的各个组件的“组件名称”、“所属服务”、“组件状态”，亦可对单个组件进行操作“启动”、“停止”、“重启”、“删除”等控制，如下图所示：

![usdp_console_node_component_management](../../images/operate/node/usdp_node/usdp_console_node_component_management.png)



