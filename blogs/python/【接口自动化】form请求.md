---
title: 【接口自动化】form请求
date: 2023-03-30
sidebar: auto
categories:
 - 测试
tags:
 - 接口自动化
---

## 什么时候使用form请求 / form请求特点 ？

- 数据量不大
- 数据层级不深的情况
- 通常以键值对传递



## form请求和json请求

> data与json传入参数的区别，通过抓包工具查看发现：
>
> - data参数传入的信息的Content-Type为application/x-www-form-urlencoded
> - json参数传入的信息的Content-Type为application/json

```python
class TestReq:
    def setup_class(self):
        self.proxy = {"http": "http://127.0.0.1:8888", "https": "http://127.0.0.1:8888"}

    def test_data(self):
        data = {"howarts": "school"}
        # 通过data参数传入请求体信息
        r = requests.post("https://httpbin.testing-studio.com/post", data=data,
                          proxies=self.proxy, verify=False)
        print(r.json())

    def test_json(self):
        data = {"howarts": "school"}
        # 通过json参数传入请求体信息
        r = requests.post("https://httpbin.testing-studio.com/post", json=data,
                          proxies=self.proxy, verify=False)
        print(r.json())
```

可以发现，通过json传递的信息，返回是由data部分返回参数，请求headers的content-type是application/json，并非通过form表单的形式传递数据。

![通过json参数传递](http://cdn.derrick51.cn/img/blog/image-20230330165142146.png)

而通过form表单传输数据时，form的数据在请求的body中，可以明显观察到与json传递不同。

![通过form表单传输](http://cdn.derrick51.cn/img/blog/image-20230330165233465.png)