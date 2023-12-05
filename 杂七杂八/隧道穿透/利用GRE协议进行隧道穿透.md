


### 利用GRE协议进行隧道穿透 
GRE协议是一种应用较为广泛的路由封装协议，用于将一种网络层协议PDU封装于任一种网络层协议PDU中，就像将一个盒子放在另一个盒子中一样。GRE是在网络上建立直接点对点连接的一种方法，目的是简化单独网络之间的连接。该协议经常被用来构造GRE隧道来穿越各种三层网络。下面讲一下如何利用GRE协议进行隧道穿透。<br />实验环境如图1-1所示，假设在内网信息收集中发现存活主机，通过漏洞获取到其主机的控制权限，并探测到其开放GRE协议，我们可以通过搭建GRE隧道的方式来进行内网穿透，具体实验环境如表1-1所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683794701-f6527024-7d10-42cc-a167-b0ac95bc2e81.png#)<br />图1-1 GRE协议实验拓扑图 <br />表1-1 GRE协议隧道穿透实验环境表

| 主机类型 | 外网IP | 内网IP | GRE隧道IP |
| --- | --- | --- | --- |
| VPS服务器  | 47.94.168.41 | 192.168.0.128 | 192.168.5.1 |
| Linux受控主机 | 123.56.14.177 | 172.16.0.1 | 192.168.5.2 |

1）首先我们需要对VPS服务器和受控主机分别执行modprobe ip_gre命令来加载ip_gre模块，执行完毕后，我们再执行lsmod | grep gre命令确认是否已加载GRE协议模块，实验操作命令如图1-2所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683794962-c0ffc6e1-4fd1-4d41-ba55-32c860372fb7.png#)<br />图1-2 加载ip_gre模块

1. 当上述ip_gre模块加载成功后,我们在VPS服务器中执行ip tunnel add tun1 mode gre remote 123.56.14.177 local 192.168.0.128命令来创建名为tun1的GRE隧道，之后通过执行ip link set tun1 up mtu 1400命令启动名为tun1的GRE隧道，此时已设定数据包最大的传输为1400字节，实验操作执行命令如图1-3所示。

![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683795223-b0c09ff6-d03e-4be7-a8bf-3faf48160aab.png#)<br />图1-3 创建启动GRE隧道tun1<br />    3）通过在VPS服务器中执行ip addr add 192.168.5.1 peer 192.168.5.2 dev tun1命令为VPS服务器创建配置双方互联IP地址，其中本端GRE隧道互联IP地址为192.168.5.1，对端GRE隧道互联IP地址为192.168.5.2，随后执行route add -net 172.16.0.0/18 dev tun1命令来创建一条到达Linux受控主机所属网段172.16.0.0/18的路由，最后通过执行echo 1 > /proc/sys/net/ipv4/ip_forward命令来开启路由转发功能，相关操作执行命令如图1-4所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683795417-c0ecf071-45da-4ad7-810e-187aecc93cf1.png#)<br />图1-4  VPS侧GRE隧道配置操作执行命令<br />   4）此时通过route -n命令来查看路由信息，如图1-5所示，可以到看到已配置的路由规则。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683795698-a81f0a6b-a3ea-4e1e-8af2-ff3c2121a602.png#)<br />图1-5  VPS服务器侧GRE隧道路由信息<br />    5）接下来我们执行类似操作，为Linux受控主机创建名为tun2的GRE隧道，之后通过执行ip link set tun2 up mtu 1400命令，启动名为tun2的GRE隧道，相关执行操作命令如图1-6所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683795926-1fec0ae4-e835-45b5-9ddc-d3b80e615906.png#)<br />图1-6 创建启动GRE隧道tun2<br />    6) 同理，我们也需要在Linux受控主机侧执行ip addr add 192.168.5.2 peer 192.168.5.1 dev tun2命令来配置双方互联IP地址，其中本端GRE隧道互联IP地址为 192.168.5.2，对端GRE隧道互联IP地址为192.168.5.1，并执行route add -net 192.168.0.0/24 dev tun2命令来创建一条到达VPS攻击服务器所属网段192.168.0.0/24的路由，最后通过执行echo 1 > /proc/sys/net/ipv4/ip_forward命令来开启路由转发功能，相关操作执行命令如图1-7所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683796137-40f76fc3-02b0-48be-a131-721f4bbffabe.png#)<br />图1-7 Linux受控主机侧GRE隧道配置操作执行命令<br />7）通过在Linux受控主机中执行route -n命令来查看路由信息，如下图1-8所示，可以到看到已配置的路由规则。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683796417-ab4c9b4a-f724-4d40-a6f8-2c3a13190094.png#)<br />图1-8  Linux受控主机侧GRE隧道路由信息<br />8）当完成以上配置后进行验证，在VPS服务器中对Linux受控主机的GRE隧道IP执行Ping操作，反过来在Gre2中进行相同验证操作。验证成功，证明GER隧道成功，如图1-9所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683796730-fe54c836-1ccf-4073-85c8-ef21a62be646.png#)<br />图1-9  GRE隧道连接测试成功

