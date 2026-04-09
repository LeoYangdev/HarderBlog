---
title: 【Web自动化】Pytest框架
date: 2023-03-23
sidebar: auto
categories: 
 - 测试
tags:
 - Pytest
---

> 前言：pytest简单易上手，支持多线程、顺序控制，兼容unittest，定制化插件开发。

## Pytest做什么？

---

- pytest 能够支持简单的单元测试和复杂的功能测试;
- pytest 可以结合 Requests 实现接口测试;结合Selenium、Appium 实现自动化功能测试;
- 使用 pytest 结合Allure 集成到Jenkins 中可以实现持续集成。
- pytest支持315种以上的插件

## 安装

---

```
pip install pytest
```

## 示例

---

```py
# test_demo.py
def add(x):
    return x + 1


def test_answer():
    assert add(5) == 6
```

### 执行

`pytest test_demo.py`即可执行

## 命名规则

----

| 类型        | 命名                    |
| :---------- | ----------------------- |
| 文件名      | test_ 开头或 _test 结尾 |
| 类名        | Test 开头               |
| 方法/函数名 | test_ 开头              |

> 测试类中不能添加 `__init__` 构造函数，否则将不被识别

## Pycharm配置环境

---

先安装pytest，然后在 “ **文件-设置-工具-Python集成工具** ” 中的测试项，更改为pytest

![image-20230323164044540](http://cdn.derrick51.cn/img/blog/image-20230323164044540.png)

## 测试用例基本结构

---

```py
class Testxxxx:
    def setup(self):
        # 资源准备
        pass
    def teardown(self):
        # 资源销毁
        pass
    def test_xxx(self):
        # 测试步骤
        # 断言
        # 断言实际结果是否等于预期结果
        assert ActResult == ExpectResult
```

## 测试用例断言

---

> 简单来说就是完成结果的校验

**第一种: assert <表达式>**

**第二种: assert <表达式>，<描述>**

```python
# @FileName: test_assert.py

def test_a():
    a = 1
    b = 3
    expect = 3
    assert a + b == expect, f"{a}+{b}=={expect},结果为真"
```

res

```cmd
FAILED                                            [100%]
AssertionError: 1+3==3,结果为真
4 != 3

预期:3
实际:4
<点击以查看差异>
```

