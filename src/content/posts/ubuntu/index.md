---
title: linux & ubuntu
published: 2025-12-28T04:24:25.185Z
---


1. win D 切回桌面。

2. ctrl alt t 打开终端。

3. deb包安装完成之后，删了deb也可以？

    .deb 本质上只是一个安装包（归档文件），安装时会把文件解压并拷贝到系统目录（如 /usr/bin、/usr/lib、/etc 等），所以删掉 .deb 完全没问题。

    deb包用sudo apt install ./xxxx 后，就出现在Applications目录了，可以直接打开了。

4. mv opencv_fractal_markers_test ~/Desktop，常用命令之mv。


5. ps aux | grep python | grep video.

    好用。但是我grep用不太熟。

6. ps -ef 和 ps aux区别？？？

    没区别。

7. pkill -f px4 ，好用。

-f：全称 --full，表示匹配进程的完整命令行字符串（而不是只匹配进程名）。

8. ls ~/Desktop/ | grep -i px4.

忽略大小写。

9. scp ~/Desktop/test.py rasp4@192.168.124.22:~

10. scp 的端口是？

    SCP（Secure Copy Protocol）的默认端口是 22。
这是因为 SCP 完全基于 SSH 协议运行，所有数据传输和认证都通过 SSH 通道进行，而 SSH 的标准端口就是 22（TCP）。

11. 传统 FTP（纯 FTP）在今天几乎是“危险品”级别——它把用户名、密码、文件内容全部明文传输，任何网络嗅探工具都能轻松截获。现代浏览器（如 Chrome、Firefox、Edge）早在几年前就彻底移除 FTP 支持了。

    2026年了，SFTP（SSH File Transfer Protocol）才是主流，它基于 SSH（端口 22），单通道、全加密（包括命令、认证、数据），防火墙友好，安全性更高，兼容性极强，几乎所有现代服务器和客户端都原生支持。

12. 在 Ubuntu 上下载并安装 FileZilla 超级简单！

    FileZilla 是目前最受欢迎的免费开源 FTP/SFTP 客户端之一，超级适合用来安全传输文件（包括通过 SSH 的 SFTP 方式）。它支持 Windows、macOS 和 Linux，界面直观，拖拽上传下载超方便。

   