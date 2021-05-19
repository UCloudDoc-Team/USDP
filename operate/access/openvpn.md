
# 配置openvpn访问USDP集群

为保障云端大数据集群环境安全，防止受到来自互联网的攻击、病毒等威胁而带来的侵害和数据安全隐患，建议您在使用云端服务时，能对云端环境配置相对严苛的安全防护措施，此问题不容忽视。

为便于开发者、维护人员等能便捷的对云端大数据环境操作和管理，我们推荐您在云端与您的办公内网/PC端之间，搭建VPN服务，可按以下步骤，在绑定EIP的UHost主机安装openvpn服务，下面以centos7操作系统示例：

## VPN服务端

### 1. 环境准备

[创建一台UHost云主机](https://docs.ucloud.cn/uhost/newuser/briefguide)及[创建一个EIP](https://docs.ucloud.cn/unet/eip/guide)，并为UHost云主机绑定EIP，为防火墙开放1194端口，选择udp协议，此处参见[防火墙操作指南](https://docs.ucloud.cn/unet/firewall/guide)

### 2. VPN安装包及脚本下载

从github下载自动部署脚本（使用其中的openvpn-install.sh脚本），git地址：`https://github.com/Nyr/openvpn-install`

  ```
    git clone https://github.com/Nyr/openvpn-install.git   
  ```
### 3. 配置VPN服务

直接运行 openvpn-install.sh脚本

  ``` 
    cd openvpn-install
    sh openvpn-install.sh
  ```

这里会自动识别eip的地址，如果需要修改对外server的IP，可以手动输入，如果使用默认，直接回车：  

```
Welcome to this OpenVPN road warrior installer!

This server is behind NAT. What is the public IPv4 address or hostname?
Public IPv4 address / hostname [10.75.37.68]: 
```
选择vpn server的协议类型，输入1是udp，2是tcp，默认使用udp，如果选择默认，直接回车：
```
Which protocol should OpenVPN use?
   1) UDP (recommended)
   2) TCP
Protocol [1]: 
```
执行完后，查看openvpn启动是否正常。

vpn port设置，如果选择默认，可以直接回车
```
What port should OpenVPN listen to?
Port [1194]: 
```

vpn dns设置，如果选择默认，可以直接回车：
```
Select a DNS server for the clients:
   1) Current system resolvers
   2) Google
   3) 1.1.1.1
   4) OpenDNS
   5) Quad9
   6) AdGuard
DNS server [1]: 
```
创建server的同时，也会给创建一个客户端文件，输入客户端文件名（这里测试生成一个client-test.ovpn的文件）：

```
Enter a name for the first client:
Name [client]: client-test
```
一路回车下去，会生成client.ovpn文件

```
Write out database with 1 new entries
Data Base Updated

client added. Configuration available in: /root/client.ovpn
```
## VPN客户端

把server部署时生成的客户端用户文件，拷贝到本地。

### MAC端

下载安装Tunnelblick[客户端下载](https://tunnelblick.net/downloads.html)

将client.ovpn拖到Tunnelblick就可以访问vpn server。

### Windows端

可以直接在openvpn官网下载客户端，选择自己操作系统对应的版本[客户端下载](https://openvpn.net/community-downloads/)

通过openvpn上点击import profile选择client.ovpn，把对应的信息加载进去

### Liunx端

安装openvpn

```
yum install -y openvpn
```

将key和client.ovpn放在同一目录下，并在该目录下执行启动命令：

```
openvpn --daemon --config client.ovpn
```

## 大数据客户端

### 配置hosts

确保服务端和客户端里，集群的所有节点的主机名和IP相匹配，便于客户端通过浏览器访问大数据组件提供的原生UI服务，如HDFS UI、Spark UI等，需在客户端hosts文件中添加如下示例内容，格式：“集群节点内网ip	主机名”

    10.10.229.204 usdp-2gz2ny-core3 
    10.10.240.154 usdp-2gz2ny-master2 
    10.10.227.236 usdp-2gz2ny-core2 
    10.10.222.134 usdp-2gz2ny-core1 
    10.10.243.138 usdp-2gz2ny-master1
