---
title: 【接口自动化】json 响应断言
date: 2023-03-29
sidebar: auto
categories:
 - 测试
tags:
 - 接口自动化
---

## 结构化响应断言JSON XML

### Json断言

```py
def test_hogwarts(self):
    url="https://home.testing-studio.com/categories.json"
    r = requests.get(url)
    assert r.json()['category_list']['categories'][0]['name'] == '提问区'
```

### Json path断言

```py
def testhogwartsjsonpath(self)
    url="https://home.testing-studio.com/categories.json"
    r = requests.get(url)
    assert jsonpath(r.json(), '$..name')[0] == '提问区'
```

#### jsonpath的类型

<img src="http://cdn.derrick51.cn/img/blog/image-20230329113142196.png" alt="image-20230329113142196" style="zoom:50%;" />
