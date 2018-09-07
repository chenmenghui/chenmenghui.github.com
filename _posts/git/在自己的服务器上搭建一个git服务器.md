---
title: 在自己的服务器上搭建一个git服务器
date: 2018-09-07
categories: git
tags: git
---

# 配置篇
## 在服务器上配置git仓库
### 安装git(ubuntu root)
apt install git
### 创建一个git用户
adduser git
### 初始化一个仓库
1. 指定一个目录作为git仓库
2. git init /srv/yii.git
### 创建证书登录
1. 创建/home/git/.ssh/authorized_keys(注意:.ssh目录及authorized_key文件是自建的)
2. 收集需要登陆用户的公匙,导入到上个创建的文件中.一个公匙占一行
## 客户端配置git
### 安装git命令行 
- ubuntu
    = apt install git
- win
    - (安装)[https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git]
### 配置git用户数据
```
// 用户信息
git config --global user.name '***'
git config --global user.email '***'
// 生成密匙
ssh-keygen // 注意会提示保存在哪里,一会要到对应的地方找,默认是在对应用户目录下的.ssh/里
```
### 把用户信息导致服务器
把公匙信息复制到服务器的/home/git/.ssh/authorized_keys中
# 使用篇
## 服务器
服务器不能直接查看用户所提交的数据,需要使用git clone [对应仓库目录]把数据提取到当前目录(pwd所在地)
## 客户端
1. git clone git@47.104.189.68:/srv/yii.git (如果没有加入公匙,也可使用git用户对应的密码)
2. 填写数据
3. git add -all (add)[https://www.cnblogs.com/skura23/p/5859243.html] 把新创建的文件提交到暂缓区
4. git commit -am '注释' 把修改内容保存到缓存区并提交至**本地仓库**
5. git push origin master 把本地仓库提交至服务器
6. git pull origin master 把服务器内容拉取下来

# 注意篇
## 如果发生冲突
```
// 和线上代码保持一致,也就是放弃上一次push之前的所有修改
git fetch -all
git reset --hard
```
## git clone 提示端口不对
可能原因
1. 服务器没有安装openssh_server(我被这个折腾了快一个小时)
2. 防火墙拦截(一般不会,默认是22端口)
