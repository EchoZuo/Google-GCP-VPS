## 用 Google GCP 搭建专属免费 VPS
## 简介
#### 直接切入正题，原理是租一个国外的服务器 --->这个服务器上搭建自己的VPN ---> 通过这个VPN。
#### 当然，既然说了免费，当然可以免费！这里就要感谢谷歌提供的GCP服务（Google Cloud Platform，谷歌云平台服务），首次注册激活GCP，你会获享12个月试用期和$300 赠金，是不是免费？

> 请注意：教程很详细，图很多！

![111.png](http://note.youdao.com/yws/res/17912/WEBRESOURCEcf8a4c155e4776b02778f386f6e00ec0)

### 首先你需要准备：
- ##### 谷歌账号以及能访问站点的一台电脑（首先你要能登录谷歌GCP）
- ##### 外币卡（VISA、Mastercard、AE）（实测招行VISA全币种卡可以）



## 开始搭建
### 1. 登录 [Google Cloud Platform](https://cloud.google.com/) 激活账户填写相关信息
- ##### 右上角点击language修改语言为中文简体（如果你的英文很棒那么可以不用改）
- ##### 点击右上角免费试用
- ##### 国家/地区可以选择香港，点击继续，登录谷歌账户

![image](http://note.youdao.com/yws/res/17669/EA11520BE44943639A5C2F4C137B4041)

- ##### 填写个人信息和信用卡信息
> 账户类型记得选个人

> 信用卡必须填写。填写之后点击开始免费使用。

> 谷歌声称“我们要求您提供信用卡信息是为了确认您不是机器人。除非您手动升级为付费帐号，否则我们不会向您收费。”具体信息可以参考：https://www.youtube.com/watch?v=kJHKsOywfxE

![2.png](http://note.youdao.com/yws/res/17691/WEBRESOURCE0051efca03d10ac59495171ed65de45a)

- 激活成功之后你会看到如下界面，并且会有提示$300美金和12个月免费试用期。

![3.png](http://note.youdao.com/yws/res/17702/WEBRESOURCE1f9736fd2126a8081f15dbb3d0d955c0)


### 2. 创建VM虚拟机实例
- ##### 左侧导航栏选择 计算 ---> Computer Engine ---> VM实例

![4.png](http://note.youdao.com/yws/res/17717/WEBRESOURCE505b34392fc9f2709b10829c537537c7)

- 等待 Compute Engine 启用完成后，点击创建，开始自定义实例配置

![5.png](http://note.youdao.com/yws/res/17724/WEBRESOURCE2b8d4b75ea367ec7206caf71e18c7c31)


    - 填写 名称
    - 选择 区域 及 地区（日本站点距离近，响应速度快）
    - 调整一下 内核 及 内存，选择最低共享配置即可满足
    - 更改 启动磁盘 为 Ubuntu 18.04 LTS

![6.png](http://note.youdao.com/yws/res/17744/WEBRESOURCEdaf8003c7c42980750ba814755d51354)

    - 防火墙勾选 允许 HTTP 流量 允许 HTTPS 流量
    - 点击 创建，创建完成如下图。

![7.png](http://note.youdao.com/yws/res/17750/WEBRESOURCEb0cfa68573cad4366553e8c718d50c39)


### 3. 设置防火墙规则
- ##### 左侧菜单栏 ---> 网络 ---> VPC网络 ---> 防火墙规则

![8.png](http://note.youdao.com/yws/res/17767/WEBRESOURCE9f9f951ce95dca8dcda9897a4ad1ba2e)

- ##### 点击创建防火墙规则

![9.png](http://note.youdao.com/yws/res/17770/WEBRESOURCE5983d7b9fa3d7d01b1b2f67fe801f77c)

    - 名称 按规则自定义
    - 目标 选择  网络中的所有实例

![10.png](http://note.youdao.com/yws/res/17778/WEBRESOURCEc5d4a0d79c4c80d42299585458ed5c8e)

    - 来源ip地址范围 0.0.0.0/0
    - 协议和端口 勾选 UDP 填写 500,4500，勾选 其他协议 填写 esp
    - 点击 创建，创建成功，如下图
    
![12.png](http://note.youdao.com/yws/res/17782/WEBRESOURCEcbf181d06b51b7354fd8ed4b7953046e)


### 4. 配置VPC网络

![13.png](http://note.youdao.com/yws/res/17789/WEBRESOURCE33b74e09e416a9c1582518a6de4520ff)

    - 左侧菜单栏 ---> 网络 ---> VPC网络 ---> default


![14.png](http://note.youdao.com/yws/res/17793/WEBRESOURCE3f29453aa17b48fc81a9697ee2288408)

    - 点击修改
    
![15.png](http://note.youdao.com/yws/res/17795/WEBRESOURCEc72b513a21c695bdbf00c826750e40b8)
    
    - 子网创建模式 选择 自动
    - 动态路由模式 选择 全局
    - 点击保存即可

### 5. 设置 负载平衡

![16.png](http://note.youdao.com/yws/res/17804/WEBRESOURCEdd61e3c456c4156dfbb7f446ac1135e6)

- ##### 左侧菜单栏 ---> 网络 ---> 负载平衡
- ##### 点击创建负载平衡
- ##### 直接点击继续


![17.png](http://note.youdao.com/yws/res/17811/WEBRESOURCE3c541fd82ebde41fb16b91ab541bfc2d) 

    - 名称 按规则自定义填写
    - 选择 区域
    - 选择第一步所创建的VM实例
    - 点击前端配置

![18.png](http://note.youdao.com/yws/res/17821/WEBRESOURCEcf35dcbafcbbe15a2a0f8e1c49b9bee4)

    - 名称 按规则自定义填写
    - 端口 填写500-4500
    - 点击 完成 
    - 点击 创建，创建完成如下

![19.png](http://note.youdao.com/yws/res/17825/WEBRESOURCE52f47094487531fec8dc158c594d50f5)

### 6. 登录服务器
- ##### 导航菜单 → Compute Engine → VM实例

![20.png](http://note.youdao.com/yws/res/17830/WEBRESOURCE7ec67b2fc7c3373971f2c2d14a2009a1)

- ##### 点击 SSH，会进入到远程终端页面

![21.png](http://note.youdao.com/yws/res/17836/WEBRESOURCE4776fdf64f4fa7ed27dac82b2384d756)


### 7. 开始部署VPN服务

- ##### 执行以下命令
```
sudo apt-get update && apt-get dist-upgrade
```
###### 控制台输出
```
The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
zuoqianheng@instance-1:~$ sudo apt-get update && apt-get dist-upgrade
Hit:1 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic InRelease
Get:2 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]               
Get:3 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]             
Get:4 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic/universe amd64 Packages [8570 kB]               
Get:5 http://archive.canonical.com/ubuntu bionic InRelease [10.2 kB]                                              
Get:6 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]                                       
Get:7 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic/universe Translation-en [4941 kB]
Get:8 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic/multiverse amd64 Packages [151 kB]              
Get:9 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic/multiverse Translation-en [108 kB]        
Get:10 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [682 kB]           
Get:11 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/main Translation-en [251 kB]      
Get:12 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [970 kB]       
Get:13 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/universe Translation-en [293 kB]  
Get:14 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [6640 B]     
Get:15 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-updates/multiverse Translation-en [3556 B]
Get:16 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-backports/main amd64 Packages [2512 B]         
Get:17 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-backports/main Translation-en [1644 B]    
Get:18 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [3736 B]     
Get:19 http://asia-northeast1.gce.archive.ubuntu.com/ubuntu bionic-backports/universe Translation-en [1696 B]
Get:20 http://archive.canonical.com/ubuntu bionic/partner amd64 Packages [2320 B]                                 
Get:21 http://archive.canonical.com/ubuntu bionic/partner Translation-en [1276 B]
Get:22 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [450 kB]
Get:23 http://security.ubuntu.com/ubuntu bionic-security/main Translation-en [156 kB]
Get:24 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [570 kB]
Get:25 http://security.ubuntu.com/ubuntu bionic-security/universe Translation-en [185 kB]
Get:26 http://security.ubuntu.com/ubuntu bionic-security/multiverse amd64 Packages [4008 B]
Get:27 http://security.ubuntu.com/ubuntu bionic-security/multiverse Translation-en [2060 B]
Fetched 17.6 MB in 4s (4193 kB/s)                     
Reading package lists... Done
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
zuoqianheng@instance-1:~$ 
```

- ##### 命令行一步部署VPN服务
```
wget https://git.io/vpnsetup -O vpnsetup.sh && sudo \
VPN_IPSEC_PSK='预分享密钥' \
VPN_USER='用户名' \
VPN_PASSWORD='密码' sh vpnsetup.sh
```
    记得修改命令中的个人信息部分。
    一个安全的 IPsec PSK 应该至少包含 20 个随机字符。
    
###### 需要等待大概5-10分钟后，控制台输出

![22.png](http://note.youdao.com/yws/res/17884/WEBRESOURCEdecae4fe5644af60870fcdb727e736e3)

> 详细部署VPN服务教程可参考：[IPsec VPN 服务器一键安装脚本](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/README-zh.md#重要提示)

- ##### 看到上图，我们的的VPN就算搭建成功了
```
================================================
IPsec VPN server is now ready for use!
Connect to your new VPN with these details:
Server IP: 34.*******
IPsec PSK: ********************
Username: ******************
Password: ***************
Write these down. You'll need them to connect!
Important notes:   https://git.io/vpnnotes
Setup VPN clients: https://git.io/vpnclients
================================================
zuoqianheng@instance-1:~$ 
```



### 8. 如何连接？
- Mac、iOS设置可以直接在网络设置中找到VPN，添加对应的服务器、用户名、密码等数据即可访问。Windows 用户可以使用免费的 [Shrew Soft 客户端](https://www.shrew.net/download/vpn)。可以参考这里：[配置 IPsec/XAuth VPN 客户端](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-xauth-zh.md)
- 如果无法连接,请首先检查是否输入了正确的 VPN 登录凭证。



## Info
- Github：https://github.com/EchoZuo
- Email: zuoqianheng@foxmail.com || QQ: 615125175
- 简书：@EchoZuo
- CSDN:https://blog.csdn.net/zuoqianheng

## 参考链接

- [Google Cloud](https://cloud.google.com/)
- [【BIG东东】关于谷歌云的结算和防火墙规则端口开通](https://www.youtube.com/watch?v=kJHKsOywfxE)
- [如何利用谷歌云搭建12个月免费VPN教程，google2018免费VPN送300美金](https://www.youtube.com/watch?v=nfIyLB3OeBU)
- [Google Cloud Platform 搭建個人VPN](https://medium.com/@YasuoYuHao/google-cloud-platform-搭建個人vpn-358ccdbeca40)
- [10分钟教你用 Google Cloud Platform 搭建自己的VPN](https://elephantnose.github.io/2018/09/24/10分钟教你用%20Google%20Cloud%20Platform%20搭建自己的VPN/#防火墙规则)
- https://github.com/hwdsl2
