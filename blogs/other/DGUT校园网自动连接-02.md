---
title: DGUT校园网自动连接-02
date: 2023-10-14
sidebar: auto
categories:
 - Other
tags:
 - DGUT
---

> 本文介绍DGUT校园网自动连接程序，适用于Windows系统。保持网络连接对于在宿舍架构服务器或云盘具有重要意义，故发布此文章。

## 原理

此程序每隔一秒请求dns验证网络是否正常，不正常进行请求，获取当前网络设备参数信息，然后再将此信息发包模拟登录的方式进行认证。

## 设置步骤

----

1、首先新建一个文本文档，打开并编辑输入以下代码。

```python
import argparse
import asyncio
import requests
import re
import urllib
import rsa
import socket
import time
import datetime
import sys

requests.packages.urllib3.util.ssl_.DEFAULT_CIPHERS = 'DEFAULT:@SECLEVEL=1'

# rsa加密函数
def rsa_no_padding(password):
    modulus = "94dd2a8675fb779e6b9f7103698634cd400f27a154afa67af6166a43fc26417222a79506d34cacc7641946abda1785b7acf9910ad6a0978c91ec84d40b71d2891379af19ffb333e7517e390bd26ac312fe940c340466b4a5d4af1d65c3b5944078f96a1a51a5a53e4bc302818b7c9f63c4a1b07bd7d874cef1c3d4b2f5eb7871 "
    exponent = "10001"
    m = int(modulus, 16)
    e = int(exponent, 16)
    t = password[::-1].encode('utf-8')
    input_nr = int.from_bytes(t, byteorder='little')

    key = rsa.PublicKey(m, e)

    # 加密过程
    crypt_nr = rsa.core.encrypt_int(input_nr, key.e, key.n)
    length = rsa.common.byte_size(key.n)
    crypt_data = crypt_nr.to_bytes(length, byteorder='big')
    return crypt_data.hex()


def request_login_url():
    requests.packages.urllib3.disable_warnings()
    r = requests.get("http://www.baidu.com", timeout=60)
    r.encoding = r.apparent_encoding
    res = r.text
    res_url = ''.join(re.findall("top.self.location.href='(.*)'</script>", res))
    r.close()
    print("请求登录url: " + res_url)
    return res_url


# 请求method login
async def request_login(userid: str = None, origin_password: str = None):
    try:
        url = request_login_url()
        mac = ''.join(re.findall("&mac=(.*)&t", url))
        password = rsa_no_padding(origin_password + '>' + mac)
        queryString = urllib.parse.quote(''.join(re.findall("index.jsp\?(.*)", url)))
        data = {
            'userId': userid,
            'password': password,
            'service': '',
            'queryString': queryString,
            'operatorPwd': '',
            'operatorUserId': '',
            'validcode': '',
            'passwordEncrypt': 'true'
        }
        r = requests.post("https://login.dgut.edu.cn/eportal/InterFace.do?method=login", data=data, verify=False, timeout=60)
        r.close()
        print(r.json()['result'])
    except Exception as e:
        print("request_login_error: " + str(e))


def check_network_status():
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        sock.connect(("8.8.8.8", 53))
        sock.close()
        return True
    except socket.error as e:
        print("网络连接异常：", str(e), time.strftime("%Y年%m月%d日 %H时%M分%S秒"))
        return False


if __name__ == '__main__':
    # 此处也可设置隐藏窗口运行
    parser = argparse.ArgumentParser()
    parser.add_argument('-u', '--userid', help='用户名', default='201121202')
    parser.add_argument('-p', '--origin_password', help='密码', default='123456')
    # 获取当前日期
    today = datetime.date.today()
    # 设定终止日期
    terminate_date = today + datetime.timedelta(days=30)
    while True:
        # 判断当前日期是否等于终止日期
        if today == terminate_date:
            print("程序将在今天终止", datetime.date.today())
            sys.exit()
        if not check_network_status():
            asyncio.run(request_login(**vars(parser.parse_args())))
        # 每隔1秒检测一次网络状态
        time.sleep(1)

```



2、然后保存后修改文件名为 `Autoconnect.py`。

3、可直接运行此程序 python -u <你的学号> -p <你的密码>



## 验证

---

1、此处在校园网自服下线所有账号，运行刚才写好的程序，发现验证成功。



## 设置开机自启

---

1、新建文本文档，输入以下代码，保存后修改文件名为connect.cmd。

```cmd
Python -WindowStyle Hidden -file "Autoconnect.py"
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


