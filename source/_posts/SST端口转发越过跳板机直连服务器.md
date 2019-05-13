---
title: SST端口转发越过跳板机直连服务器
date: 2019-05-13 19:56:00
tags: 
    - network
	- ssh
---

### 1. 需求

假设服务器仅能通过跳板机进行连接，一般情况下连接服务器需要以下两步：

1. 连接跳板机：ssh user@xxx.xxx.xxx.xxx -p xx

2. 连接服务器：ssh user@serverip -p xx (此时在跳板机环境下)

如果需要传文件给服务器，则需要先传给跳板机，再传给服务器，非常麻烦。windows有一些工具可设置ssh隧道使得文件可以直接传给服务器，不过如果不想用这些ftp工具，使用putty也可以直接实现这个功能。

### 2. 步骤

- **Windows(putty)**

  1. session里HostName设置的是跳板机IP，端口为跳板机端口
  2. Connection-SSH-Tunnels, Add new forwarded port：在下方选项为Local情况下，source port填写的端口号为本机端口，destination填写服务器IP:port，点击add，添加成功会从上方forwarded ports内看到添加的转发端口。
  3. 保存session
  4. 这种方法仍然需要先登录到跳板机，之后再新建一个ssh连接到本机刚刚设置的端口，你就会发现已经登录到服务器了。

- **Mac OS/Linux**

  linux下只需要一条命令，即可直接连接服务器。

  `ssh -f -N -g -L 3000:172.20.0.7:22 user@jumpserver_ip -p 3000`

  这条命令第一个3000是本机端口，省略了127.0.0.1；172开头的IP是服务器IP，端口号22；后面是跳板机IP加端口。这条命令使得后台运行了ssh服务，然后直接ssh连接127.0.0.1:3000就可以登录服务器。



