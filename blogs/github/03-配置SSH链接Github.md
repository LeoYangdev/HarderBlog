---
title: 03-配置SSH链接Github
date: 2025-02-03
sidebar: auto
tags:
 - 教程
categories: 
 - Github入门
---

> 本文介绍如何配置ssh链接Github仓库以便实现管理，配置该功能后无需每次输入密钥即可完成仓库操作。
> 环境：windows10

## 前置操作：打开cmd或git bash

## 第一步：配置用户信息并在本机生成ssh Key

```shell
# 配置用户名和邮箱
git config --global user.name "xxx"
git config --global user.email "xxx.@xxx.com"
```

```shell
ssh-keygen -t rsa -C "xxx@xxx.com"
//上面为你的邮箱,执行后根据提示操作,一直回车也可以
```

## 第二步：生成后查看并复制key

```shell
cat ~/.ssh/id_rsa.pub
```

该rsa key内容复制后备用，如下图所示

![image-20250204121956528](http://cdn.derrick51.cn/img/blog/image-20250204121956528.png)

## 第三步：登录Github并添加公钥

> 路径：Github-Settings-SSH and GPC keys

![image-20250204122307874](http://cdn.derrick51.cn/img/blog/image-20250204122307874.png)

> 进入该界面，添加SSH Key

![image-20250204122818182](http://cdn.derrick51.cn/img/blog/image-20250204122818182.png)

![image-20250204122840919](http://cdn.derrick51.cn/img/blog/image-20250204122840919.png)

## 第四步：验证是否成功

```shell
ssh -T git@github.com
```

验证结果如下便成功配置啦！！！

![image-20250204123005290](http://cdn.derrick51.cn/img/blog/image-20250204123005290.png)

恭喜你，已经配置完成了，下面[使用bash进行仓库操作](./04-拉取和上传到Github仓库.md)吧！
