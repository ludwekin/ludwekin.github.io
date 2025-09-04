---

title : "Linux编程，内核裁切,相关笔记"

published : false

---

## Linux内核裁切是什么技术？

### Linux 内核裁切（Kernel Trimming / Kernel Slimming）是什么？

简单来说，它是 把 Linux 内核中不需要的功能模块、驱动和子系统删除或禁用，从而：
1. 减小内核体积
    - 对于嵌入式设备、IoT、路由器、工业控制器，存储空间有限，内核越小越好。
2. 提高性能和启动速度
    - 加载更少模块 → 启动更快
    - 内存占用更低 → 系统更稳定
3. 增强安全性
    - 少了不必要的模块，就少了攻击面。

### 技术实现方法
1. **配置裁剪（Config Trimming）**
    - 使用 `make menuconfig` 或 `make xconfig`  
    - 禁用不需要的功能，例如：
        - 文件系统（ext4, NTFS, XFS）  
        - 网络协议（IPv6, NFS）  
        - 外设驱动（摄像头、声卡、USB 等）  

2. **模块化（Loadable Kernel Modules）**
    - 内核功能做成模块（.ko 文件）  
    - 不用时不编译进内核，而是动态加载（insmod / modprobe）  

3. **裁切工具和补丁**
    - 使用 `Buildroot`、`Yocto` 等嵌入式 Linux 构建工具  
    - 也可以手动应用 custom patch 去掉冗余子系统  

4. **内核裁剪优化（Advanced）**
    - 去掉调试符号（strip）  
    - 静态链接只保留必要函数  
    - 移除冗余库和 API  


## Linux编程特点

1. 程序分为两大类，一个内核态，一个用户态。
