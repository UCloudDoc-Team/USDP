
# 配置openvpn

按以下步骤，可以在绑定EIP的uhost安装openvpn，下面以centos7操作系统示例：

## 1.服务端

  1： 创建一个云主机并绑定EIP，并为防火墙开放1194端口，选择udp协议
  
  2：从github下载自动部署脚本（使用其中的openvpn-install.sh脚本），git地址：`https://github.com/Nyr/openvpn-install`
  
  ```
    git clone https://github.com/Nyr/openvpn-install.git   
  ```
  3. 直接运行 openvpn-install.sh脚本
  
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
## 2.客户端

把server部署时生成的客户端用户文件，拷贝到本地。

### 2.1 MAC

下载安装Tunnelblick[客户端下载](https://tunnelblick.net/downloads.html)

将client.ovpn拖到Tunnelblick就可以访问vpn server。

### 2.2 Windows

可以直接在openvpn官网下载客户端，选择自己操作系统对应的版本[客户端下载](https://openvpn.net/community-downloads/)

通过openvpn上点击import profile选择client.ovpn，把对应的信息加载进去

### 2.3 Liunx

安装openvpn

```
yum install -y openvpn
```

将key和client.ovpn放在同一目录下，并在该目录下执行启动命令：

```
openvpn --daemon --config client.ovpn
```

## 3. 配置hosts

确保服务端和客户端里，所有节点的主机名和IP相匹配，方便在浏览器服务。例如：

    10.10.229.204 usdp-2gz2ny-core3 
    10.10.240.154 usdp-2gz2ny-master2 
    10.10.227.236 usdp-2gz2ny-core2 
    10.10.222.134 usdp-2gz2ny-core1 
    10.10.243.138 usdp-2gz2ny-master1
