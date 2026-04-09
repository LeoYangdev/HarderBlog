---
title: 03-Ubuntu 部署 CUPS打印服务器实现局域网打印共享
date: 2023-08-24
sidebar: auto
tags:
 - 配置那些事
categories: 
 - 配置操作
---

# Ubuntu 部署 CUPS打印服务器实现局域网打印共享

> 环境：
>
> - 操作系统：Ubuntu 22.04
> - 打印机：惠普 HP Deskjet 1110 Series （HP Deskjet 1112）

## 安装cups

1、打开终端（您可以使用快捷键 `Ctrl + Alt + T` 打开终端）输入以下命令，确保apt为最新版本

```shell
sudo apt update
sudo apt upgrade
```

2、安装命令（Ubuntu 桌面会预装CUPS服务）

```shell
sudo apt install cups
```

3、配置CUPS服务

```shell
# 启动服务
sudo systemctl start cups
sudo systemctl enable cups
# 查看状态
sudo systemctl status cups
```

4、更改配置文件，打开打印机浏览功能

```shell
# 使用 nano 打开配置文件
sudo nano /etc/cups/cupsd.conf
```

- 找到 `Browsing Off` 更改为 ``

- 查找并修改 `Listen` 选项：取消注释 `Listen` 选项并确保它的值为 `Listen *:631`。这将允许来自任何 IP 地址的远程连接访问 CUPS Web 管理界面。
- 将 `Allow @LOCAL` 改为 `Allow all `以允许远程访问，参照下方修改

```shell
Browsing On
#######
<Location />
  Order allow,deny
  Allow @LOCAL
</Location>
#######
<Location /admin>
  AuthType Default
  Require valid-user
  Order allow,deny
  Allow @LOCAL
</Location>
```

5、按下 `ctrl + x` 键，然后按下 `Y` 键以保存所做的更改。最后，按下 `Enter` 键以退出文本编辑器。

6、重启 CUPS 服务：为了使更改生效，需要重启 CUPS 服务。

```
sudo systemctl restart cups
```

7、更改防火墙配置（可选）

```shell
sudo ufw allow 631/tcp
```

8、打开 ip:631 验证服务是否启动

## 添加打印机

通过 CUPS Web 管理界面，您可以添加和配置打印机。在界面的首页上，点击 "Administration"，然后选择 "Add Printer"。按照界面上的指导添加和配置您的打印机。

![image-20230824214643584](http://cdn.derrick51.cn/img/blog/image-20230824214643584.png)

> 注意，如果进入管理员页面需要输入用户名和密码，请使用系统（用户）账户密码登录。

![image-20230824214253697](http://cdn.derrick51.cn/img/blog/image-20230824214253697.png)

### 选择对应打印机添加

![image-20230824214725494](http://cdn.derrick51.cn/img/blog/image-20230824214725494.png)

![image-20230824214820268](http://cdn.derrick51.cn/img/blog/image-20230824214820268.png)

添加后，在printers页面查看到打印机就正常了

### 打印测试页面

在打印机选项内，可以打印测试页，若打印成功，证明您已成功部署 cups 服务器。

![image-20230824214940929](http://cdn.derrick51.cn/img/blog/image-20230824214940929.png)

## 局域网打印

### 获取打印机地址

由于cups使用ipp通信，我们需要获取打印机地址，在 `ip:631` 页面中可以查看。

![image-20230824215424547](http://cdn.derrick51.cn/img/blog/image-20230824215424547.png)

### Windows系统添加打印机

![image-20230824215717958](http://cdn.derrick51.cn/img/blog/image-20230824215717958.png)

选择驱动，安装完成后打印测试页，若成功即可使用。

### Android系统添加打印机

1、部分机型可直接在系统服务添加，在系统打印输入地址即可添加。

```shell
打印服务器ip:631/printers/打印机名称
```

2、下载 CUPS Print ，然后按上面的步骤添加，选择 CUPS Print 服务

 CUPS Print：[下载链接](https://f-droid.org/zh_Hans/packages/io.github.benoitduffez.cupsprint/)

3、一般局域网打印机会扫描出来，如果没有的话就点右上角输入

4、打印时若打印机无法选择，可以选择其它打印机，再选择即可。

![image-20230824220620386](http://cdn.derrick51.cn/img/blog/image-20230824220620386.png)