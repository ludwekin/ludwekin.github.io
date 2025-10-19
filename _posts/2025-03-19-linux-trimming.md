---

title : "Linux Trimming"

published : true

---



### Linux Kernel Trimming / Slimming Definition

In simple terms, Linux kernel trimming is the process of removing or disabling unnecessary kernel modules, drivers, and subsystems, resulting in:

1. **Reduced kernel size**  
   - For embedded devices, IoT, routers, and industrial controllers, storage is limited, so a smaller kernel is better.

2. **Improved performance and boot speed**  
   - Fewer modules to load → faster startup  
   - Lower memory usage → more stable system

3. **Enhanced security**  
   - Fewer unnecessary modules → smaller attack surface。

### Technical Implementation Methods

1. **Config Trimming**  
   - Use `make menuconfig` or `make xconfig`  
   - Disable unnecessary features, e.g.:  
     - Filesystems (ext4, NTFS, XFS)  
     - Network protocols (IPv6, NFS)  
     - Peripheral drivers (camera, sound card, USB, etc.)

2. **Loadable Kernel Modules (LKM)**  
   - Implement kernel features as modules (.ko files)  
   - Don’t compile them into the kernel; load dynamically when needed (`insmod` / `modprobe`)

3. **Trimming Tools and Patches**  
   - Use embedded Linux build systems like `Buildroot` or `Yocto`  
   - Apply custom patches to remove redundant subsystems manually

4. **Advanced Kernel Trimming / Optimization**  
   - Strip debug symbols (`strip`)  
   - Static linking retaining only necessary functions  
   - Remove redundant libraries and APIs




### Characteristics of Linux Programming

Programs are divided into two main categories: **kernel space** and **user space**.




### POSIX API

**POSIX API** refers to a set of system calls and library functions that comply with the **POSIX (Portable Operating System Interface)** standard.  
- **Purpose:** Ensure portability across Unix-like systems (Linux, macOS, BSD, etc.).  
- **Provides:** Access to core operating system functionality.

#### Common POSIX API Function Categories

1. **Process Management**  
   - `fork()`, `exec()`, `wait()`, `exit()`

2. **File and Directory Operations**  
   - `open()`, `read()`, `write()`, `close()`, `stat()`, `mkdir()`

3. **Device I/O**  
   - All devices are abstracted as files, accessed via `read()` and `write()`

4. **Memory Management**  
   - `mmap()`, `munmap()`

5. **Signal Handling**  
   - `signal()`, `sigaction()`, `kill()`

6. **Threads (POSIX Threads, pthreads)**  
   - `pthread_create()`, `pthread_join()`

7. **Inter-Process Communication (IPC)**  
   - Pipes `pipe()`, message queues, shared memory, semaphores

8. **Socket Networking**  
   - `socket()`, `bind()`, `listen()`, `accept()`, `connect()`


### kernal

linux distribution like Ubuntu don't have kenal codes.

