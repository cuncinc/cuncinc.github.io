---
date: 2023-8-16
tag: 折腾
status: public
title: 通过ssh隧道使用Windows远程桌面
---

在内网中，出于安全因素，微软的RDP协议被禁用，哪怕就在一个局域网中，却不能使用Windows远程登录连接桌面，只能使用其他软件如向日葵之类的来连接。

## ssh隧道

后来看到可以使用Cloudflare的Tunnel来中转，我试过后发现速度太慢，延迟高达390ms，实在无法忍受。于是想到，隧道技术不就是把A协议封装在B协议中吗？这应该是很成熟的技术才对。

于是我在网络上找到了[SSH隧道技术](https://blog.csdn.net/qwe123321123/article/details/116504970)的文章，就是把RDP封装在SSH上，我早已经把ssh server配好，直接可以使用：

```bash
ssh username@server.example.com -p 22 -L 3001:localhost:3389 -N -o ServerAliveInterval=60 -o ServerAliveCountMax=3 -f
```

最重要的两个参数是`-L`后的端口号，第一个`3001`是本地映射过来的端口，第二个`3389`是服务器要映射的端口。

最后打开远程桌面连接，输入映射过来的端口即可使用。延迟仅1ms，极致丝滑的享受。

![](https://pic.chunson.cc/remote-connect.png)

使用ssh隧道要求控制端和被控端必须能够直连，若二者在不同的局域网中，都没有公网IP，则可以使用内网穿透工具连接。

## 使用frp中转连接

[frp](https://gofrp.org/)是一个内网穿透工具，分为服务器端frps和客户端frpc，服务器端运行在公网服务器上，可以将内网的端口暴露在公网环境，因此你需要一台云服务器才能使用此工具。

首先，在服务器上下载好frps，在客户端下载frpc，并分别新建名为`frps.ini`和`frpc.ini`的配置文件，其配置如下：

```ini
; frps.ini
; assume that server's ip is 111.222.111.222
[common]
bind_port = 12345
authentication_method = token
token = your_token
dashboard_port = 12346
dashboard_user = your_username
dashboard_pwd = your_password
```

```ini
; frpc.ini
[common]
server_addr = 111.222.111.222
server_port = 12345
authetication_method = token
token = your_token

[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389

[file_transport]
type = tcp
local_ip = 127.0.0.1
local_port = 445
remote_port = 445

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 522
```

以上配置中，`bind_port`是frp通信使用的端口，两个配置文件中必须一致；`token`用于身份认证，亦必须一致；`dashboard_port`是frps控制台的端口，在浏览器上使用111.222.111.222:12346打开。

需要穿透的端口在frpc.ini中配置，每一个服务使用`[]`标识，每个括号的内容自定义，不相同即可。

运行之：

```bash
frps -c frps.ini	#in server
frpc -c frpc.ini	#in client
```

成功运行后，在远程桌面连接使用`111.222.111.222:3389`即可连接上远程桌面，其延迟和带宽取决于云服务器。

