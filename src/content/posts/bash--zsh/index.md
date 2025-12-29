---
title: bash & zsh
published: 2025-12-25T03:21:03.692Z
---



为什么 .sh.template 能运行?

因为文件里面写的是 合法的 shell 语法。

扩展名只是为了人类阅读方便，Linux 判断能否执行主要靠 权限和内容，所以 .sh.template 也能运行。

./xxxx.sh 的时候，后面还可以加入参数，这是怎么做到的？

./xxxx.sh arg1 arg2。

脚本可以获取这些参数。这个功能是 shell 脚本的内置机制。

在 shell 脚本中，命令行传入的参数会自动被 shell 存到一些特殊变量里。

$1	第 1 个参数（arg1）。

$2	第 2 个参数（arg2）。

arg1=${1:-default_value}  ，如果没传就用 default_value。

echo $0 。

tail -f filename.

默认情况下，tail filename 会显示文件的最后 10 行内容。

加 -f 后，它不仅显示最后的内容，还会持续“跟随”文件的变化。

fg不带参数,把最近一个后台作业带到前台。

./xxxx.sh 2>&1 &，这里的 & 不是后台符号，它的作用是 告诉 shell 后面是一个文件描述符，而不是文件名。

sh文件里面，怎么添加代码，让这个sh执行的时候也执行xxx.sh???

其实就是想在一个 .sh 脚本里 调用另一个脚本（xxx.sh），让它在自己执行的时候也被执行。

scp -r orangepi@172.16.10.101:/home/orangepi/ws/log_folder ~/Downloads/

这个scp速度还是挺快的。









