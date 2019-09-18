---
title: "Qnap配置WebDAV"
date: 2019-02-28
tags: [Qnap,WebDAV,NAS]

---

# Qnap配置WebDAV

1. 打开控制台进入Web服务器配置页面，选择启动Web服务器
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513395846630.jpg)

2. 进入webDAV选项卡，打开启用WebDAV。权限选择WebDAV权限。
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513393831152.jpg)


3. 转到 权限/共享文件夹 配置页面，点击 编辑共享文件夹权限
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513404261107.jpg)

4. 权限类别选择WebDAV访问，访问权限和用户根据需要选择
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513395238565.jpg)

5. 点击应用就配好了。

下面我演示使用DEVONthink链接WebDAV服务器

1. 选择 Add WebDAV Server
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513398596538.jpg)

2. URL粘贴Qnap中启动的WebDAV服务地址。用户名密码是刚才选择的用户
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513399077846.jpg)

3. 选择Ok后可以看到已经开始同步，配置成功。
![](https://img.geyuxu.com/blog-Nas-20190228-Qnap配置WebDAV.md.15513399232557.jpg)


