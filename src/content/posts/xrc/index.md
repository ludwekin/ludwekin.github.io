---
title: 复刻xrc遥控器
published: 2026-01-23T09:02:38.701Z
---

xrc4.2的pcb由三块板子组成，分别是主板，电源板和通道板。

我把群里面的xrc4.2的这三个pcbdoc文件倒入了嘉立创eda，开始复刻。

按照群文件所述，xrc4.2和xrc4.1的主板是通用的，那就是说xrc4.2改动了电源板和通道板。

## 记载我遇到的一些问题和解答

1. xrc4.2项目，群文件里面提供了xrc.axf文件和xrc.bin文件，那么axf文件和bin文件区别？

    AXF = ARM eXtended Format。

    用途是调试用。

2. bin文件可以烧录为bootloader吗？或者只能烧录为完整的程序？

    这个问题问得不太好，因为我不太熟悉这方面。

    .bin 的本质就是按地址顺序摊平的 Flash 数据。或者说，“从某个起始地址开始，要往 Flash 写的字节流”。

    如果.bin文件的链接地址在 Flash 起始或者这个文件烧在了 boot 区域，这时候，这个 .bin 就是 bootloader。

    对于有 bootloader 的系统，大概率就是bootloader.bin是启动用的。app.bin就是主程序了。

3. xrc4.2项目里面，出现了xrc.axf和xrc.hex，这两个文件的区别？

    .hex 文件是什么？本质就是ASCII 文本文件。

    .hex文件和.bin文件的区别之一就是.hex是带地址信息的。

