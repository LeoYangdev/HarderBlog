---
title: 【Web自动化】自动化测试定位策略
date: 2023-03-28
sidebar: auto
categories:
 - 测试
tags:
 - web自动化
---

> 初学自动化测试，一般会遇到以下几个问题，这里针对自动化测试的定位做出策略判断，更好的做出决策。
>
> - 不知道应该使用哪种定位方式
> - 元素定位不到如何解决

## 根据场景选择定位方式

---

除了`selenium`提供的八大定位，还可以使用`js/css`定位

<img src="http://cdn.derrick51.cn/img/blog/image-20230328153626711.png" alt="image-20230328153626711" style="zoom:50%;" />

## 选择定位器通用原则

> 按优先级排列，从上往下优先级递减

1. 与研发约定的属性优先(class属性：[name='locate'])
2. 身份属性id ,name(web定位)
3. 复杂场景使用组合定位：

- xpath, css
- 属性动态变化(id， text)
- 重复元素属性(id, text, class)
- 父子定位(子定位父亲)

4. js定位

## web弹窗定位

---

场景：web页面alert弹框

解决：web需要使用`driver.switchTo().alert()`处理

## 下拉框/日期控件定位

---

场景：

- `<input>`标签组合的下拉框无法定位
- `<input>`标签组合的日期控件无法定位

解决：

- 面对这些元素，我们可以用JS注入解决问题

## 文件上传定位

---

场景：

- input标签文件上传

解决：

- input标签直接使用send_keys()方法
