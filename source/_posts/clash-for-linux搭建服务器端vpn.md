---
title: clash for linux搭建服务器端vpn

---

### 1. 项目介绍

此项目是通过使用[clash](https://link.zhihu.com/?target=https%3A//github.com/Dreamacro/clash)作为核心程序，再结合脚本实现简单的代理功能。

主要是为了解决我们在服务器上下载GitHub等一些国外资源速度慢的问题。

### 2. 下载 clash-for-linux

文档地址：https://github.com/wanhebin/clash-for-linux

```bash
$ git clone https://github.com/wanhebin/clash-for-linux.git
```

进入到项目目录，编辑start.sh脚本文件，修改变量URL的值,此值为Clash订阅地址。

clash订阅地址需自行购买，推荐[wgetcloud](https://3jkkvi9afjjln2yjwnbc.wgetcloud.org/)

```bash
$ cd clash-for-linux
$ vim start.sh
```

### 3. 启动程序

直接运行脚本文件start.sh

进入项目目录

```bash
$ cd clash-for-linux
```

运行启动脚本

```bash
$ sh start.sh
Clash订阅地址可访问！                                      [  OK  ]
配置文件config.yaml下载成功！                              [  OK  ]
服务启动成功！                                             [  OK  ]

Clash Dashboard 访问地址：http://IP:9090/ui
Secret：xxxxxxxxxxxxx

请执行以下命令加载环境变量: source /etc/profile.d/clash.sh

请执行以下命令开启系统代理: proxy_on

若要临时关闭系统代理，请执行: proxy_off
```

命令行登陆

```bash
$  curl http://192.168.10.29:9090/ui -H "Authentication: b&ZlKTte5OnEt2Sn"
$ source /etc/profile.d/clash.sh
$ proxy_on
```

检查服务端口

```bash
$ netstat -tln | grep -E '9090|789.'
tcp        0      0 127.0.0.1:9090          0.0.0.0:*               LISTEN     
tcp6       0      0 :::7890                 :::*                    LISTEN     
tcp6       0      0 :::7891                 :::*                    LISTEN     
tcp6       0      0 :::7892                 :::*                    LISTEN
```

检查环境变量

```bash
$ env | grep -E 'http_proxy|https_proxy'
http_proxy=http://127.0.0.1:7890
https_proxy=http://127.0.0.1:7890
```

以上步骤如果正常，说明服务clash程序启动成功，现在就可以体验高速下载github资源了。

### 4. 停止程序

进入项目目录

```bash
$ cd clash-for-linux
```

关闭服务

```bash
$ sh shutdown.sh
```

服务关闭成功，请执行以下命令关闭系统代理：proxy_off

```bash
$ proxy_off
```

然后检查程序端口、进程以及环境变量http_proxy|https_proxy，若都没则说明服务正常关闭。

### 5. Clash Dashboard

访问 Clash Dashboard 通过浏览器访问 start.sh 执行成功后输出的地址，例如：`http://192.168.0.1:9090/ui`

登录管理界面 在API Base URL一栏中输入：`http://IP:9090` ，在`Secret(optional)`一栏中输入启动成功后输出的Secret。

点击Add并选择刚刚输入的管理界面地址，之后便可在浏览器上进行一些配置。

更多教程 此 Clash Dashboard 使用的是[yacd](https://link.zhihu.com/?target=https%3A//github.com/haishanh/yacd)项目，详细使用方法请移步到yacd上查询。

### 6. 使用须知

- 此项目不提供任何订阅信息，请自行准备Clash订阅地址。
- 运行前请手动更改start.sh脚本中的URL变量值，否则无法正常运行。
- 当前在RHEL系列和Debian系列Linux系统中测试过，其他系列可能需要适当修改脚本。
- 支持 `x86_64/aarch64` 平台
