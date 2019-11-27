---
title: Git和Github之间的一些操作
date: 2019-11-26 12:45:55
categories: Git
tags:
---

## 配置SSH

### 什么是SSH

SSH是一种协议标准,其目的是实现安全远程登录以及其它安全网络服务。我们使用SSH就相当于是使用一个身份凭证，通过密钥可以免密登陆。可以将电脑端与Github账号连接起来。

### 配置步骤

1. 获取密钥：`$ ssh-keygen-t rsa-C "your_email@youremail.com"`输入后提示操作路径以及密码，直接回车跳过即可。
2. 从显示路径中获取出密钥：C:\Users\ASUS\.ssh 下的id_rsa.pub文件，打开后复制其中全部内容。
3. 打开Github，找到账户设置中的SSH keys，新建SSH key，随便输入一个title，然后将所有内容粘贴到内容之中。
4. 输入ssh -T git@github.com 可以查看状态。显示 Hi hxk0912! You've successfully authenticated, but GitHub does not provide shell access. 就可以证明绑定成功。


##　上传代码到Github

1. 先在Github网站上新建一个仓库，然后再本地init一个本地仓库
2. 在本地仓库中bash，输入git remote add origin <仓库的url>
3. 然后 add -> commit操作
4. 最后 git push -u origin master 即可

### 遇到的问题

#### 问题

在最后一步提交过程中提示仓库与本地不符，提交失败。

#### 原因

这是因为我在新建仓库的时候选择了新建readme文件，初始化本地仓库的时候没有readme文件，这就造成了不相符。

#### 解决方法

在push提交之前先使用git pull -rebase origin master同步即可

**注意这里必须要用rebase来解决冲突问题。**
