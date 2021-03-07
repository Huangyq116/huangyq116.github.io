---
title: Linux设置网络延迟丢包操作
permalink: 
date: 2019-07-30 20:37:00
categories:
  - 2.Programming
  - Shell
tags:
  - Shell
---

## 1.tc方式

\* 清除设备策略：tc qdisc del root dev eth2 2>/dev/null
\* 设置设备策略：tc qdisc add dev eth0 root netem loss 5%

tc qdisc add dev eth2 root netem loss 5%
tc qdisc add dev eth2 root netem delay 200ms
tc qdisc add dev eth2 root netem delay 200ms loss 5%
tc qdisc add dev eth2 root netem delay 400ms



## 2.comcast方式

https://github.com/tylertreat/comcast



## 3.iptables方式