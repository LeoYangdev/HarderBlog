---
title: DGUT校园网自动连接
date: 2023-03-17
sidebar: auto
categories:
 - Other
tags:
 - DGUT
---

> 本文介绍DGUT校园网自动连接程序，适用于Windows系统。保持网络连接对于在宿舍架构服务器或云盘具有重要意义，故发布此文章。

## 设置步骤

----

1、首先新建一个文本文档，打开并编辑输入以下代码。

```powershell
while (1){
set str =$(ping -n 4 -w 80 baidu.com)`
echo $str
$result=[regex]::matches($str,'100% 丢失')

if($result.success)
{
echo 怎么又没网了？发包
####此处用下面复制的powershell代码替换####
sleep 2`
}
else{
sleep 2`
}
}
```



2、右击下方 **`InterFace.do?method=login`** 的网址，选择复制为 **`powershell`** 备用，输入到上面新建的文本文档，然后保存后修改文件名为 `Autoconnect.ps`。

![image-20230317000034636](http://cdn.derrick51.cn/img/blog/image-20230317000034636.png)

![image-20230317001048286](http://cdn.derrick51.cn/img/blog/image-20230317001048286.png)

## 验证

---

1、此处在校园网自服下线所有账号，运行刚才写好的程序，发现验证成功。

![image-20230317001844404](http://cdn.derrick51.cn/img/blog/image-20230317001844404.png)

## 设置开机自启

---

1、新建文本文档，输入以下代码，保存后修改文件名为connect.cmd。

```cmd
PowerShell.exe -WindowStyle Hidden -file "Autoconnect.ps1"
```

2、右击开始菜单，进入计算机管理，单击创建基本任务，填入名称及描述。

<img src="http://cdn.derrick51.cn/img/blog/image-20230317104120009.png" alt="image-20230317104120009" style="zoom:50%;" />

3、设置触发器（当计算机启动时）

<img src="http://cdn.derrick51.cn/img/blog/image-20230317104400932.png" alt="image-20230317104400932" style="zoom:50%;" />

4、操作选择启动程序

<img src="http://cdn.derrick51.cn/img/blog/image-20230317104426912.png" alt="image-20230317104426912" style="zoom:50%;" />

5、按图示填入对应路径，选择你刚才创建的cmd文件，起始目录为该文件所在目录。

![image-20230317104528452](http://cdn.derrick51.cn/img/blog/image-20230317104528452.png)

6、点击完成即可。





