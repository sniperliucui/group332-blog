---
title: rpa robort
tags: rpa
categories: others
toc: true
copyright: true
reward: false
top: false
mathjax: true
date: 2023-01-14 01:00:00
---

RPA（Robotic Process Automation），译为流程自动化机器人，又可以称为数字化劳动力（Digital Labor），是一种智能化软件，通过模拟并增强人类与计算机的交互过程，实现工作流程中的自动化。

<!--more-->

RPA（Robotic Process Automation），译为流程自动化机器人，又可以称为数字化劳动力（Digital Labor），是一种智能化软件，通过模拟并增强人类与计算机的交互过程，实现工作流程中的自动化。

RPA 具有对企业现有系统影响小，基本不编码，实施周期短，而且对非技术的业务人员友好等特性。RPA 不仅可以模拟人类，而且可以利用和融合现有各项技术如规则引擎、光学字符识别、语音识别、机器学习及人工智能等前沿技术来实现其流程自动化的目标。

## 如何学习

- [下载链接](https://gde.huawei.com/#/group/weautomate_rpa/weautomate_rpa_download?type=download)

- 文档
    在软件的帮助菜单内有文档，但文档只描述每个模块的输入输出和整体下载配置，并不教怎么用

- [课程](https://connect.huaweicloud.com/courses/learn/course-v1:HuaweiX+CBUCNXEA001+Self-paced/about)
    华为自己推出的课程，可以使用

- [论坛](https://bbs.huaweicloud.com/forum/forum-1407-1.html)
    能查询一些遇到的问题

## 界面

### 控件

![图1.png](..%2Fimages%2Frpa/图1.png)
针对不同情况可以选择不同控件，控件有许多大类，这里介绍使用到的几个。

- 流程控制
  ![图2.png](..%2Fimages%2Frpa/图2.png)
  流程控制就是循环、分支、顺序这3个结构，加上调用子脚本和异常处理。

- 公共
  ![图3.png](..%2Fimages%2Frpa/图3.png)
  这里主要使用文本处理，这部分就是字符串的匹配、查找、合并等操作。加解密就是提供了一种加解密方式；编程调试部分可以调用python写好的脚本；调用部分可以调用shell、c#等其他文件。

- 应用
  ![图4.png](..%2Fimages%2Frpa/图4.png)
  应用模块目前只支持这几类应用，内部分别是各个应用的读取和写入。

## 使用
    不开放api第三方应用操作，通常是保密性质的东西。
在这里展示在桌面上新建一个txt文件，后写入某excel内容，可以将文件夹类比为不开放api的第三方应用。
![图5.png](..%2Fimages%2Frpa/图5.png)
![图6.png](..%2Fimages%2Frpa/图6.png)
