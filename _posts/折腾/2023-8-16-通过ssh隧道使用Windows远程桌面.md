---
date: 2023-8-16
tag: 折腾
status: public
title: 通过ssh隧道使用Windows远程桌面
---

在学校的内网中，出于安全因素，微软的RDP协议被禁用，哪怕就在一个局域网中，却不能使用Windows远程登录连接桌面，只能使用其他软件如向日葵之类的了连接。

后来看到可以使用Cloudflare的Tunnel来中转，我试过后发现速度太慢，延迟高达390ms，实在无法忍受。于是想到，隧道技术不就是把A协议封装在B协议中吗？这应该是很成熟的技术才对。

于是我在网络上找到了[SSH隧道技术](https://blog.csdn.net/qwe123321123/article/details/116504970)的文章，就是把RDP封装在SSH上，我早已经把ssh server配好，直接可以使用：

```bash
ssh username@server.example.com -p 22 -L 3001:localhost:3389 -N -o ServerAliveInterval=60 -o ServerAliveCountMax=3 -f
```

最重要的两个参数是`-L`后的端口号，第一个`3001`是本地映射过来的端口，第二个`3389`是服务器要映射的端口。

最后打开远程桌面连接，输入映射过来的端口即可使用。延迟仅1ms，极致丝滑的享受。

![](https://pic.chunson.cc/remote-connect.png)