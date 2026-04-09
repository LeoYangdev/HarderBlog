---
title: 【Web自动化】强制等待与隐式等待
date: 2023-03-27
sidebar: auto
categories:
 - 测试
tags:
 - web自动化
---

## 为什么要添加等待？

---

- 避免页面未渲染完成后操作，导致的报错
- 要在报错的元素行前添加等待

## 强制等待

---

解决方案:在报错的元素操作之前添加等待

原理:强制等待，线程休眠一定时间

演练环境: [霍格沃兹测试开发 (ceshiren.com)](https://vip.ceshiren.com/#/layout/section)

```py
def wait_sleep():
    driver = webdriver.Chrome()
    driver.get("https://vip.ceshiren.com")
    # 不加等待，可能会因为网速等原因产生报错
    # ************ 强制等待的使用
    # ============ 报错:no such element: Unable to locate element
    # ============ 原因: 页面未加载完成，就去查找元素，此时这个元素还没加载出来
    # ============ 解决方案: 在no such element: Unable to locate element报错之前添加强制等待，等待页面汽染完成
    # 如果没有报错，证明就是页面渲染速度导致得问题，如果添加了强制等待还报错，那么可能是别的问题，比如定位错误
    time.sleep(3)
    driver.find_element(By.XPATH, "//*[text()='个人中心']")
```



## 隐式等待

---

问题: 难以确定元素加载的具体等待时间。

解决方案:针对于寻找元素的这个动作，使用隐式等待添加配置。

演练环境: [霍格沃兹测试开发 (ceshiren.com)](https://vip.ceshiren.com/#/layout/section)

原理: 设置一个等待时间，轮询查找(默认0.5秒)元素是否出现，如果没出现就抛出异常

```py
#设置一个等待时间，轮询查找《默认0.5秒)元索是否出现，如果没出现就抛出异常
driver.implicitly_wait(3)
```

```py
def wait_sleep():
    driver = webdriver.Chrome()
    driver.get("https://vip.ceshiren.com")
    # *********隐式等待
    # 强制等待的问题:
    # 1. 不确定页面的加载时间,可能会因为等待时间过长，而影响用例的执行效率
    # 2. 不确定页面的加载时间,可能会因为等待时间过短,而导致代码依然会报错
    # 注意: 1、在代码一开始运行的时候就添加隐式等待的配置,注意,隐式等待是全局生效,所以在所有的find_erement动作之前就执行此代码
    # 2、隐式等待只能解决元素查找的问题，不能解决元素不可交互的问题
    driver.implicitly_wait(3)
    driver.find_element(By.XPATH, "//*[text()='个人中心']")
```

### 调试获取断点信息

隐式等待一定要在查找前使用

![image-20230326221404066](http://cdn.derrick51.cn/img/blog/image-20230326221404066.png)



### 隐式等待解决不了的问题

- 元素可以找到使用点击等操作，出现报错
- 原因:
- 1. 页面元素加载是**异步加载**过程通常html会先加载完成，js、css其后
  2. 元素存在与否是由HTML决定，元素的交互是由css或者is决定
  3. 隐式等待只关注元素能不能找到，不关注元素能否点击或者进行其他的交互
- **解决方案**:使用**显式等待**

```py
# 例子：隐式等待解决不了的问题
def wait_show():
    driver = webdriver.Chrome()
    driver.get("https://vip.ceshiren.com/#/ui_study/frame")
    driver.implicitly_wait(5)
    # 问题：元素可以找到，但效果无法触发
    driver.find_element(By.ID, "success_btn").click()
    # time.sleep(5)
```



## 显式等待

---

示例:  WebDriverWait(driver实例，最长等待时间，轮询时间).until(结束条件)

原理: 在最长等待时间内，轮询，是否满足结束条件

演练环境: [霍格沃兹测试开发 (ceshiren.com)](https://vip.ceshiren.com/#/ui_study/frame)

注意: 在初级时期，先关注使用

```py
def wait_until():
    driver = webdriver.Chrome()
    driver.get("https://vip.ceshiren.com/#/ui_study")
    # 问题: 元素可以找到，但是点击效果缺没有触发
    # 原因:
    # 第一个参数是driver ，第二个参数是最长等待时间，until方法内需要结合expected_conditions或者自己封装的方法进行使用
    # expected_conditions的参数传入的都是一个元组,即多一层小括号
    WebDriverWait(driver, 10).until(expected_conditions.element_to_be_clickable((By.ID, "success_btn")))
    driver.find_element(By.ID, "success_btn").click()
    time.sleep(5)
```

## 总结

---

| 类型     | 使用方式                                                    | 原理                         | 使用场景                                       |
| -------- | ----------------------------------------------------------- | ---------------------------- | ---------------------------------------------- |
| 直接等待 | time.sleep(等待时间)                                        | 强制线程等待等               | 调试代码，临时性添加                           |
| 隐式等待 | driver.implicitly_wait(等待时间)                            | 在时间范围内，轮询查找元素   | 解决找不到元素的问题，无法解决交互问题         |
| 显式等待 | WebDriverWait(driver示例,等待时间,轮询时间).until(结束条件) | 设定特定的等待条件，轮询操作 | 解决特定条件下的等待问题，比如点击等交互性行为 |

> 更多内容查阅selenium官方文档，selenium官方文档：[Waits | Selenium](https://www.selenium.dev/documentation/webdriver/waits/)

