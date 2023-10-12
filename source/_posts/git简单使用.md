---
title: git简单使用
categories: git
tags:
- git
---

## 创建一个GitHub项目

[Git学习笔记](https://www.it235.com/%E5%AE%9E%E7%94%A8%E5%B7%A5%E5%85%B7/Git/git.html#git%E5%AE%89%E8%A3%85)

![img](http://myblog.over2022.top/a0b171f9d76b452990f87f018a138fcc.png) 
给项目取个名字
![img](http://myblog.over2022.top/ce116f1ae7a042319e1d1485e1c8833a.png) 
创建成功后页面
![img](http://myblog.over2022.top/6b0f78851383465ea83319921550f910.png) 

## 使用git上传项目到github

在本地机器上进入要上传项目的文件目录，右键->Git Bash Here 
![img](http://myblog.over2022.top/53880535006e41989121efc09a6a9f65.png)


```sh
git init #初始化Git仓库，将此文件夹作为git仓库
git add .  #将项目上所有的文件添加到仓库中
git commit -m "first commit"  #表示你对这次提交的注释
git remote add origin https://自己的仓库url地址	#将本地的仓库关联到github上
git push -u origin master  #把代码上传到github仓库
```

## git常见命令与问题

### 本地仓库重新设置源

```sh
git commit -m "Change repo." # 先把所有为保存的修改打包为一个commit
git remote remove origin # 删掉原来git源
git remote add origin [YOUR NEW .GIT URL] # 将新源地址写入本地版本库配置文件
git push -u origin master # 提交所有代码
```

### 解决 fatal: unable to access 'https://github.com/.../.git': Could not resolve host: github.com

```sh
#取消代理设置
git config --global --unset http.proxy 
git config --global --unset https.proxy
```

有时候是因为网络被墙的原因，网速慢，重试几次就成功

## 从git下载项目

```sh
# 初始化
git init
# 加入远程仓库
git remote add origin [远程仓库地址]
# 下载代码
git clone [远程仓库地址]
```

版本回退命令：`git reset --HARD head^`

本文参考：

[git常用命令](https://blog.csdn.net/halaoda/article/details/78661334?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166515319516782388085854%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166515319516782388085854&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-78661334-null-null.142^v51^control_1,201^v3^add_ask&utm_term=git%E5%91%BD%E4%BB%A4&spm=1018.2226.3001.4187)
[在github上上传本地项目步骤（两种方式）](https://blog.csdn.net/oNew_Lifeo/article/details/115189603?ops_request_misc=&request_id=&biz_id=102&utm_term=github%E4%B8%8A%E4%BC%A0%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-115189603.nonecase&spm=1018.2226.3001.4187)
[常见问题](https://blog.csdn.net/weixin_44713763/article/details/104640842)
