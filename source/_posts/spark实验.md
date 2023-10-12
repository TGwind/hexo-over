---
title: spark实验
---

# 一、安装ubutun16.04

[ubuntu](http://www.javaxxz.com/thread-365251-1-1.html)安装好后，[root](http://www.javaxxz.com/thread-346489-1-1.html)初始[密码](http://www.javaxxz.com/thread-287552-1-1.html)（默认密码）不知道，需要设置。

1、先用安装时候的用户登录进入系统

2、输入：sudo passwd  按回车

3、输入新密码，重复输入密码，最后提示passwd：password updated sucessfully

此时已完成root密码的设置

[安装一个vim](https://blog.csdn.net/weixin_43622586/article/details/109108917#:~:text=%E4%BA%8C%E3%80%81%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95%201%200.%E5%AE%89%E8%A3%85%20sudo%20apt-get%20install%20vim%201,%3Aq%20%E9%80%80%E5%87%BA%E7%A8%8B%E5%BA%8F%20%E4%B8%89%E3%80%81%E4%B8%BE%E4%BE%8B%EF%BC%8C%E4%BB%A5three-gpp-http-example.cc%E4%B8%BA%E4%BE%8B%20%E8%BE%93%E5%85%A5%20sudo%20vim%20scratch%2Fthree-gpp-http-example.cc%20)

> ​	sudo apt-get install vim

尽量不使用vi，这两种编辑方式不同

## 1、关闭防火墙

```powershell
#查看防火墙状态
systemctl status firewalld 
#永久关闭防火墙
systemctl disbale firewalld
```

安装ssh服务

```
sudo apt install openssh-server
```

在安装完成后，SSH服务将自动启动，并在系统启动时自动运行。您可以通过以下命令检查SSH服务的运行状态：

```
sudo service ssh status
```

连接xshell时[ssh服务器拒绝密码解决方法](https://blog.csdn.net/qq_42768347/article/details/108851552)

3）打开ssh服务器的配置文件，输入命令：

>  vi /etc/ssh/sshd_config

4）在弹出窗口中找到Authentication，使用vi命令修改其中PermitRootLogin后的prohibit-password为yes（如果不会使用vi命令，参考下面的注意）

重新启动ssh服务器，输入命令：**sudo /etc/init.d/ssh restart**

## 安装jdk

要安装Java Development Kit (JDK) 8，您可以按照以下步骤进行操作：

```
sudo apt install openjdk-8-jdk
```

```
java -version
```

## 安装hadoop

要安装Hadoop 3.1.3，您可以按照以下步骤进行操作：

1. 在Hadoop官方网站（[https://hadoop.apache.org/releases.html ↗](https://hadoop.apache.org/releases.html)）上下载Hadoop 3.1.3的二进制分发版本。您可以选择下载tar.gz或者zip格式的文件。

2. 解压下载的文件。在终端中，导航到下载的文件所在的目录，并执行以下命令解压文件（以tar.gz为例）：

   shell

   Copy

   ````
   tar -xzf hadoop-3.1.3.tar.gz
   ```
   这将解压缩Hadoop文件到当前目录。
   ````

3. 将解压缩的Hadoop目录移动到适当的位置。例如，您可以将其移动到`/usr/local`目录下：

   shell

   Copy

   ````
   sudo mv hadoop-3.1.3 /usr/local/hadoop
   ```
   这将把Hadoop文件移动到`/usr/local/hadoop`目录。
   ````

4. 6 ） 将 Hadoop 添加到环境变量
   （1）获取 Hadoop 安装路径
   [leokadia@hadoop102 hadoop-3.1.3]$ pwd
   /opt/module/hadoop-3.1.3
   （2）打开/etc/profile.d/my_env.sh 文件
   [leokadia@hadoop102 hadoop-3.1.3]$ sudo vim /etc/profile.d/my_env.sh

5. 进入Hadoop配置目录：

   shell

   Copy

   ````
   cd /usr/local/hadoop/etc/hadoop
   ```
   ````

6. 配置Hadoop。在该目录下，您可以编辑以下文件来配置Hadoop：

   - core-site.xml：配置Hadoop核心设置。
   - hdfs-site.xml：配置Hadoop分布式文件系统（HDFS）设置。
   - yarn-site.xml：配置Hadoop资源管理框架（YARN）设置。
   - mapred-site.xml：配置Hadoop MapReduce设置。

   根据您的需求和环境，修改这些配置文件。至少，您需要设置一些基本的配置，如文件系统的URI和Hadoop集群的节点信息。

7. 格式化HDFS。在终端中运行以下命令以格式化HDFS：

   shell

   Copy

   ````
   hdfs namenode -format
   ```
   
   这将初始化HDFS并创建必要的目录和文件。
   ````

8. 启动Hadoop。运行以下命令启动Hadoop集群：

   shell

   Copy

   ````
   start-dfs.sh
   start-yarn.sh
   ```
   
   这将启动HDFS和YARN服务。
   ````

现在，您已经成功安装和配置了Hadoop 3.1.3。您可以使用Hadoop命令和API进行大数据处理和分布式计算。请注意，根据您的需求，您可能还需要进行其他配置和调整。阅读Hadoop官方文档和其他资源，以了解更多详细的配置和使用说明。
