---
title: 通用分布式计算引擎Ray
tags: Ray
categories: 分布式
toc: true
---

[Ray](https://docs.ray.io/en/latest/) is an open-source unified framework for scaling AI and Python applications. It provides the compute layer for parallel processing so that you don’t need to be a distributed systems expert.
<!--more-->

# 分布式


# Ray安装
Ray currently officially supports x86_64, aarch64 (ARM) for Linux, and Apple silicon (M1) hardware. Ray on Windows is currently in beta.

```bash
pip install -U "ray[data,train,tune,serve]"

# For reinforcement learning support, install RLlib instead.
# pip install -U "ray[rllib]"
```
