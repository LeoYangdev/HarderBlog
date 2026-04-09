---
title: 莞工e志愿-校园志愿服务与勤工俭学管理
date: 2025-02-05
sidebar: auto
tags:
 - PHP
 - ThinkPHP
 - 微信小程序
 - DGUT
categories: 
 - 项目
---

![PHP](https://img.shields.io/badge/ThinkPHP-6.0-blue.svg)
![PHP](https://img.shields.io/badge/微信小程序-3.3.5-orange.svg)
![Mysql](https://img.shields.io/badge/Mysql-5.7-blue.svg)
![七牛云](https://img.shields.io/badge/七牛云存储-1.0-blue.svg)

::: tip 介绍

本系统基于ThinkPHP、MySQL、微信小程序等技术，实现了志愿服务和勤工俭学的发布、报名、审核录用、签到签退、学分转换等功能，基本涵盖活动的全流程。用户信息可通过中央认证一键同步到本系统，无需手动填写。此外，平台给用户提供扫码计时的功能，用户可通过扫描管理员提供的限时二维码进行计时；同时在管理端，给管理员提供学分转换功能，快捷的将系统志愿时长与学分导出，便于对接其它系统。
:::

## 系统架构设计

<div algin="center">
    <img src="http://cdn.derrick51.cn/img/blog/wps1.png#pic_center" alt="img"  />
</div>

## 小程序前台

### 首页浏览相关功能

用户进入小程序，默认跳转到首页，可查看活动详情，点击可进入活动详情页，进行报名等操作。

小程序首页功能界面如图6-1所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps2.jpg" alt="活动浏览功能界面">
</div>

<center>图 6-1 活动浏览功能界面</center>

###  活动页面相关功能

用户进入活动页后，可点击活动查看活动详情，并进行报名操作。

小程序活动列表界面如图6-2所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps3.jpg" alt="img">
</div>


<center>图 6-2 活动列表界面</center>

用户点击进入活动详情页，点击报名。志愿活动报名详情界面如图6-3所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps4.jpg" alt="img">
</div>

<center>图 6-3 志愿活动报名详情界面</center>

点击报名，志愿活动报名成功界面如图6-4所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps5.jpg" alt="img">
</div>

<center>图 6-4 志愿活动报名成功界面</center>

### 扫码计时相关功能

用户进入我的页面后，可显示相关个人信息和功能入口。

我的界面如图6-5所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps6.jpg" alt="img">
</div>

<center>图 6-5 我的界面</center>

进入我的页面，可点击扫码计时，小程序将调用摄像头完成签到/签退操作。

> 请注意，此扫码计时功能仅能从小程序页面进入扫码，微信扫码无效。

扫码计时功能如图6-6所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps7.png" alt="img">
</div>


<center>图 6-6 扫码计时界面</center>

扫码完成后，将跳转到新页面显示状态。

计时成功界面如图6-7所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps8.jpg" alt="img">
</div>

<center>图 6-7 计时成功界面</center>

### 个人资料修改功能

用户进入我的页面后，可点击个人资料进入个人资料详情页，并进行编辑修改操作。个人资料在编辑时，点击用户头像可上传头像到七牛云，系统将会返回头像同步URL，在页面标记完后，需要点击保存才能将数据上传，用户类型、身份证号码、中央认证系统账号均不可手动输入，均为系统同步数据。

个人资料修改界面如图6-8所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps9.jpg" alt="img">
<img src="http://cdn.derrick51.cn/img/blog/wps10.jpg" alt="img">
</div>

<center>图 6-8 个人资料修改界面</center>

### 用户反馈相关功能

用户在我的－用户反馈页面可进行反馈，用户需要选择反馈类型（反馈/建议），再输入标题、内容后才可提交反馈。否则将提示未填写错误。

在页面下方可查看历史反馈列表，卡片内可查看提交时间和处理状态，点击卡片将跳转到新页面查看详情。

用户反馈界面如图6-9所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps11.jpg" alt="img">
<img src="http://cdn.derrick51.cn/img/blog/wps12.jpg" alt="img">
</div>

<center>图 6-9 用户反馈界面</center>

### 中央认证相关功能

用户在我的－个人资料，点击中央认证账号即可进入中央认证页面，输入中央认证账号密码即可完成认证，信息将在后台进行同步。

中央认证界面如图6-10所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/image-20250205190724830.png" alt="img" style="zoom:50%;">
</div>


<center>图 6-10 中央认证界面</center>

## 小程序后台

### 后台管理首页

管理员进入后台后，可选择功能进行操作，由于篇幅限制，在此仅展示超级管理员页面所有功能。

后台管理界面如图6-11所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps14.jpg" alt="img">
</div>

<center> 图 6-11 后台管理界面</center>

### 志愿活动管理

管理员进入后台后，点击志愿活动，可进入志愿活动列表，并对志愿活动进行发布、删除、修改、查看报名名单操作。

志愿活动管理界面如图6-12所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps15.jpg" alt="img">
</div>

 <center>图 6-12 志愿活动界面</center>

报名人员名单界面如图6-13所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps16.jpg" alt="img">
</div>

 <center>图 6-13 报名人员名单界面</center>

生成签到二维码界面如图6-14所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps17.jpg" alt="img">
</div>

 <center>图 6-14 生成签到二维码界面</center>

###  部门管理模块

管理员进入后台后，点击部门管理，可进入部门列表，并对部门进行新增、删除、修改、查看操作。普通管理员只能查看部门用户列表。

部门管理界面如图6-15所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps18.jpg" alt="img">
</div>

 <center>图 6-15 部门管理界面</center>

### 权限管理模块

超级管理员进入后台后，进入权限管理页面，选择管理部门，可查看对应部门管理员、删除管理员以及搜索添加管理员。

权限管理界面如图6-16所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps19.jpg" alt="img">
<img src="http://cdn.derrick51.cn/img/blog/wps20.jpg" alt="img">
</div>

 <center>图 6-16 权限管理界面</center>

### 内容管理模块

管理员进入后台后，进入内容管理页面，可对文章进行编辑、删除、发布新文章的操作。文章发布后，将展示在往期活动列表，管理员文章列表也将同步更新。

管理员仅可查看到本部门发布的文章，超级管理员才可以查看所有部门发布的文章和详细内容，并对系统所有文章进行编辑、删除。图6-16中左侧为普通管理员页面，右侧为超级管理员页面。

内容管理界面如图6-17所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps21.jpg" alt="img">
<img src="http://cdn.derrick51.cn/img/blog/wps22.jpg" alt="img">
</div>

<center>图 6-17 内容管理界面</center>

### 学分转换模块

管理员进入后台后，进入学分转换，设置学分转换比例后，点击生成下载链接，即可生成excel文档，点击复制链接，可在浏览器打开并下载。

学分转换界面如图6-18所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps23.jpg" alt="img">
</div>

<center> 图 6-18 学分转换界面</center>

### 反馈管理模块

管理员进入后台后，进入反馈管理，可查看反馈列表，并对反馈内容进行处理，点击处理将跳转到新页面进行查看，输入回复内容并提交，即可完成反馈回复。

反馈管理界面如图6-19所示：

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/wps24.jpg" alt="img">
<img src="http://cdn.derrick51.cn/img/blog/wps25.jpg" alt="img">
</div>

<center>图 6-19 反馈管理界面</center>

## 部署说明

1、在微信小程序端配置后端域名，七牛云存储bucket区域和域名。

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/image-20250205200124171.png" alt="img">
</div>
2、在后端`项目目录\egg_volunteer\config\database.php`中配置数据库相关参数，并将sql文件导入数据库。

3、在后端`项目目录\egg_volunteer\app\controller\Login.php`中配置微信小程序后台相关信息。

4、在后端`项目目录\egg_volunteer\config\qiniu.php`中配置七牛云存储相关key以及bucket。

5、在后端`项目目录\egg_volunteer\app\service\DgutService.php`中配置**request_url域名**为项目网站域名。

6、出于安全考虑，建议在后端`项目目录\egg_volunteer\app\service\AuthService.php`中修改JWT密钥。

7、注意，中央认证功能需要借助Python程序执行，需要安装的环境如下图所示。

<div align="center">
<img src="http://cdn.derrick51.cn/img/blog/image-20250205204329866.png" alt="img">
</div>

> 如无需使用中央认证及七牛云存储（头像功能），则无需进行4、5、6、7步配置，只需要配置1、2、3即可。

## 项目下载

[Github仓库链接](https://github.com/LeoYangdev/egg-ezy)

