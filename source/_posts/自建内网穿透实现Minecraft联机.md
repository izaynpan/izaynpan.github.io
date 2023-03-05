---
title: 自建内网穿透实现Minecraft联机
tag: 服务器
date: 2023-3-6
---

## 前言

跟朋友联机Minecraft我尝试过两种方法：[**樱花穿透**](https://www.natfrp.com/)、[**Essential模组**](https://essential.gg/)（这两种联机方法我仍然强力推荐）。不巧最近这两种方法在我目前的网络环境下都不稳定，频繁掉线，正巧手中有一台云服务器，这台服务器此前一直是挂着云崽（详情可见我之前的[博客](https://opano.top/2022/10/17/N1%E7%9B%92%E5%AD%90%E9%83%A8%E7%BD%B2Yunzai-Bot%E7%9A%84%E6%80%BB%E7%BB%93/)），对带宽和处理器的消耗很小，再运行个内网穿透服务完全没问题（还能帮忙消耗1000GB的月流量包，不然浪费了😉）。工具选用**frp**([项目页面](https://github.com/fatedier/frp))。

> ## 原理
>
> frp 主要由 **客户端(frpc)** 和 **服务端(frps)** 组成，服务端通常部署在具有公网 IP 的机器上，客户端通常部署在需要穿透的内网服务所在的机器上。
>
> 内网服务由于没有公网 IP，不能被非局域网内的其他用户访问。
>
> 用户通过访问服务端的 frps，由 frp 负责根据请求的端口或其他信息将请求路由到对应的内网机器，从而实现通信。
>
> *[摘自frp[官方文档](https://gofrp.org/docs/concepts/)]*

## 服务器端

**1.下载frp**

[[下载页面]](https://github.com/fatedier/frp/releases)首先在Terminal里使用`uname -s`和`uname -u`来确定服务器的系统和架构，我的云服务是Linux x86_64，就下载后缀为linux_amd64的版本：

```
wget https://github.com/fatedier/frp/releases/download/v0.47.0/frp_0.47.0_linux_amd64.tar.gz 
```

**2.编辑配置文件**

解压后，编辑**frps.ini**，内容为下：

```ini
[common]
bind_port = 7000
token = 设置一个客户端连接密码
```

`[common]` 是固定名称的段落，用于配置通用参数。

`bind_port`是服务器监听端口，默认7000，用来接收客户端的连接。

`token`用来权限验证，增加一点安全性。

至于有些教程里出现了关于dashboard的配置，如果不使用网页查看frp的状态以及统计信息展示的话，可以不配置。

## 客户端（处于内网的机器）

**1.下载frp**

[[下载页面]](https://github.com/fatedier/frp/releases)直接根据后缀名下载就行，windows、linux、darwin(MacOS)。

**2.编辑配置文件**

解压后，编辑**frpc.ini**，内容为下：

```ini
[common]
server_addr = 服务器的公网ip地址
server_port = 7000
token = 与服务器配置文件里的相同

[minecraft]
type = tcp
local_ip = 127.0.0.1
local_port = 25565
remote_port = 25565
```

`server_addr`是服务端的公网ip地址，如果有域名解析到服务器也可以直接填域名。

`server_port`与服务器端配置的`bind_port`保持相同即可。

`[minecraft]`是一个标志，可以随意命名。

`type`一定是tcp。

`local_ip` 是需要被代理的本地服务的 IP 地址，默认127.0.0.1就行。

 `local_port` 是本地需要暴露到公网的服务地址和端口，Minecraft里安装[**联机模组**](https://www.mcmod.cn/class/4498.html)后局域网一般开放25565端口（建议安装联机模组，可自定义开放端口，否则是随机分配端口，每次都要修改配置文件十分麻烦）。

`remote_port` 表示在 frp 服务端监听的端口，访问此端口的流量将会被转发到本地服务对应的端口（即`local_ip`:`local_port`），朋友在服务器地址栏里输入`server_addr`:`remote_port`便能加入游戏，建议25565。

## 云服务器开放防火墙/安全组

以腾讯云为例，在服务器实例的管理页面会有防火墙一栏，记得放行7000和25565两个端口。

<center><img src="自建内网穿透实现Minecraft联机\image.png" /></center>

## 开始联机

**1.服务器端启动frp服务**

在frp文件夹内`./frps -c ./frps.ini`启动服务，`ctrl+c`结束服务。

(frp官方文档建议长期运行时，使用systemd控制frps及配置开机自启。[教程](https://gofrp.org/docs/setup/systemd/))

**2.客户端启动frp服务**

在frp的文件夹里新建个bat文件，内容为`frpc -c frpc.ini`，双击运行启动服务，关闭窗口结束服务。

**3.游戏内操作**

房主开放局域网游戏后，朋友在服务器地址栏里输入`server_addr`:`remote_port`(例如1.111.222.233:25565)便能加入游戏，享受稳定的联机游戏体验吧~

**温馨提示**：注意自己云服务器的流量消耗，建议选择大的包月流量包或者比较便宜的按量付费，毕竟Minecraft联机真的挺吃流量的，特别是开图的时候，我自己使用下来，11个小时吃了快5GB，我的云崽机器人半个月也才吃300MB。