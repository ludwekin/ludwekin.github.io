---

title : "C++ 个人学习笔记"

---





## C++基础

1. 对象与指针：
    - `Animal a; `这段代码，a 本身就是一个 Animal 对象。a是个栈对象。
    - `Animal* p = &a; ` p 是指针，存的是 a 的地址。变量 p 里存放的是一个 地址值。这个地址指向内存中 a 所在的位置。用 *p 可以访问 a。
2. new关键字与对象与隐式转换：
    - `Animal* p = new Dog();`	这段代码，new Dog() 在堆上创建一个 Dog 对象。返回堆内存的地址 (类型是 Dog*)。隐式转为 Animal*，存到 p。	所以 p 本质上是一个“指向 Animal 的指针”。Dog对象需要用 delete 手动释放（或用智能指针）。优点是对象可以在作用域外继续存在，更灵活。



## C++ 简述与发展史

C++ 是由 Bjarne Stroustrup 在 1980 年代初期于贝尔实验室开发的编程语言。

C++ 常常被用来开发编译器和虚拟机，因为它的性能表现不错。



## 笔记

1. 标准情况**：每个 `.cpp` 文件生成 **1 个 `.obj` 文件**  。
2. 一个项目，肯定有不止一个cpp文件，这很好理解。为了让项目结构清晰，把代码按照功能分开写到不同文件里面是很正常的。

## 常见问题解答

### 1. 为什么C++项目中既有.h文件又有.cpp文件？

**头文件(.h)**
- **作用**：声明（Declaration）
- **内容**：类定义、函数声明、常量、宏等
- **目的**：告知其他文件有哪些函数、类可供使用

**源文件(.cpp)**
- **作用**：实现（Implementation）
- **内容**：函数的具体实现代码
- **目的**：编写具体逻辑，无需对外暴露实现细节

**设计原因**

这种"声明与实现分离"的编程风格源于C++的历史背景：
- C++继承自C语言，C语言在1970年代就采用了函数声明与实现分离的设计
- 相比之下，Python和JavaScript等现代语言更加灵活，可直接定义函数和类，无需头文件

---

## REST API 概述

### 起源与定义

REST（Representational State Transfer）概念由 Roy Fielding 在 2000 年的博士论文中提出。

**核心背景**：
- Roy Fielding 是计算机科学家，HTTP协议的主要设计者之一
- REST并非具体软件或产品，而是一种架构设计原则
- 旨在指导Web系统设计，使系统更简单、可扩展、易维护

### 成为主流标准的原因

1. **跨平台兼容性**
   - 前端网页、移动应用、IoT设备均可调用REST API
   - 无需使用相同编程语言

2. **简单直观**
   - 基于HTTP协议（GET/POST/PUT/DELETE）
   - URL对应资源，HTTP方法对应操作
   - 开发者易于理解和使用

3. **无状态特性**
   - 每次请求独立，服务器不保存客户端状态
   - 便于横向扩展，增加服务器数量不影响客户端

4. **良好扩展性**
   - 新资源可独立添加，不破坏现有系统
   - 系统模块化程度高

---

## C++ 面向对象特性

### 继承访问控制

```cpp
class D : public B {
    // 类实现
};
```

**继承访问级别**：
- `public`继承：B的public成员在D中保持public，protected成员保持protected
- `private`继承：B的public和protected成员在D中均变为private
- `protected`继承：B的public和protected成员在D中均变为protected

### 模板 vs 运行时多态

**模板实现（编译期决定）**

```cpp
template<typename T>
T square(T x) {
    return x * x;
}

int main() {
    int a = square(3);        // 编译器生成int版本
    double b = square(2.5);   // 编译器生成double版本
}
```

编译器会生成对应的具体函数：

```cpp
int square(int x) { return x * x; }
double square(double x) { return x * x; }
```

**运行时多态实现（虚函数）**

```cpp
struct Base {
    virtual double square(double x) = 0;
};

struct IntSquare : Base {
    double square(double x) override { 
        return (int)x * (int)x; 
    }
};

struct DoubleSquare : Base {
    double square(double x) override { 
        return x * x; 
    }
};

int main() {
    Base* p = new DoubleSquare();
    double result = p->square(2.5);  // 运行时通过vtable决定调用的函数
}
```

---

## STL 标准模板库

### STL算法

STL算法是一组用模板编写的函数，可在不同容器上执行常见操作。算法独立于具体容器，通过迭代器操作数据。

### 容器分类

容器是C++ STL的核心组件，属于类模板，可存放不同类型的对象。

**(a) 序列式容器**

数据按顺序存储：
- `std::vector`：动态数组（最常用）
- `std::list`：双向链表  
- `std::deque`：双端队列

**(b) 关联式容器**

数据按键值自动排序：
- `std::set`：不重复集合
- `std::map`：键值对（类似Python字典）
- `std::multiset/multimap`：允许重复元素

**(c) 无序容器**

基于哈希表实现：
- `std::unordered_set`
- `std::unordered_map`

**(d) 容器适配器**

将容器包装为特殊用途：
- `std::stack`：栈（后进先出）
- `std::queue`：队列（先进先出）
- `std::priority_queue`：优先队列（最大/最小堆）

---

## 函数式编程

函数式编程是一种将函数视为"一等公民"的编程范式。

**核心特点**：
- 函数可像变量一样传递、返回、组合
- 强调不可变性，数据不被修改而是产生新数据
- 使用表达式而非语句
- 代码更接近数学函数形式

**C++中的函数式编程示例**：

```cpp
#include <algorithm>
#include <vector>
#include <iostream>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};
    
    // 使用lambda表达式实现函数式风格
    for_each(v.begin(), v.end(), [](int x){ 
        cout << x * x << " "; 
    });
}
```

**Lambda表达式解析**：
- `[](int x){ cout << x * x << " "; }`
- `[]`：捕获列表（此处未捕获任何变量）
- `(int x)`：参数列表
- `{ cout << x * x << " "; }`：函数体

**Lambda特点**：
- 匿名函数，无需命名，直接在需要处使用
- 轻量级，可像变量一样传递给算法
- 支持闭包，能捕获外部变量

---

## C++多线程编程

### 多线程的必要性

1. **性能提升**
   - 充分利用多核CPU，并行执行任务
   - 示例：一个线程读取文件，另一个线程处理数据

2. **响应性改善**  
   - GUI程序中主线程保持界面流畅，后台线程执行耗时任务
   - 示例：下载器中一个线程负责下载，另一个更新进度条

3. **并行处理能力**
   - 大任务分解为多个小任务同时处理
   - 应用场景：科学计算、大数据处理、游戏物理引擎

### 基础线程创建

C++11引入`<thread>`头文件，简化了线程创建：

```cpp
#include <iostream>
#include <thread>
using namespace std;

int main() {
    thread t([](){
        cout << "Hello from another thread!" << endl;
    });
    
    t.join();
}
```

### 线程池概念

**定义**：线程池是一组预先创建的线程，等待任务分配执行。

**优势**：
- 避免频繁创建和销毁线程，减少系统开销
- 控制并发线程数量，防止系统过载
- 支持任务队列管理

**核心问题**：
1. **condition_variable的作用**：避免线程空转，减少CPU占用
2. **线程池安全关闭**：设置停止标志，通知所有线程退出
3. **任务执行顺序**：通常采用FIFO队列，也可实现优先级调度

### 动态线程池实现

**核心思路**：
- 维护线程池和任务队列
- 根据任务队列长度和空闲线程数动态调整线程数量
- 保持线程数在合理范围内（最小值到最大值）

**关键技术点**：
1. **线程增加条件**：任务积压且空闲线程不足时创建新线程
2. **线程减少条件**：线程长期空闲且总数超过最小值时销毁部分线程
3. **同步控制**：使用`std::mutex`和`std::condition_variable`避免竞态条件
4. **线程安全计数**：跟踪空闲线程数和总线程数

**示例代码结构**：

```cpp
void adjustThreadPool() {
    std::unique_lock<std::mutex> lock(mutex);
    if(tasks.size() > idleThreads && totalThreads < maxThreads) {
        // 增加线程
        workers.emplace_back([this]{ this->worker(); });
        totalThreads++;
    } else if(idleThreads > minThreads && totalThreads > minThreads) {
        // 减少多余线程
        stopExtraThreads();
    }
}
```

`std::mutex`是互斥锁，用于保护共享资源，防止多个线程同时访问导致冲突。

---

## 游戏服务器中的线程池应用

### 应用场景

线程池在游戏服务器中主要用于并发处理大量玩家请求，避免为每个请求创建独立线程。

**典型请求类型**：
- 玩家移动、攻击、技能操作
- 数据库操作（排行榜、背包数据）
- 网络消息收发

**架构优势**：
- 固定数量线程处理动态任务，节约资源
- 避免线程过多导致系统压力
- 支持异步任务处理，提升响应速度

### 典型架构结构

1. **任务队列**：网络模块解析玩家请求后，封装为函数对象放入队列
2. **工作线程**：从队列获取任务执行，实现线程复用
3. **事件驱动**：主线程监听网络事件，将事件交给线程池处理

**示例代码**：

```cpp
void onPlayerRequest(NetworkMessage msg) {
    threadPool.enqueue([msg]() {
        // 处理玩家请求，如更新位置、计算伤害
        processPlayerAction(msg);
    });
}
```

### 关键问题解答

1. **为什么不为每个玩家创建线程？**
   - 大量玩家会导致线程数过多，消耗系统资源，降低整体效率

2. **如何处理任务积压？**
   - 实现动态线程池扩展或任务优先级排队，防止服务器崩溃

3. **如何保证线程安全？**
   - 使用mutex等同步原语保护共享数据（玩家状态、地图信息等）

### 协议选择考量

游戏服务器倾向于使用二进制协议而非JSON的原因：

1. **实时性要求**：UDP数据包大小受限，JSON格式可能导致丢包
2. **性能考虑**：JSON解析需要字符串处理，CPU消耗较高  
3. **带宽限制**：大型游戏每秒需传输大量数据，二进制更高效

---

## Boost库介绍

Boost是为C++开发的开源库集合，提供经过严格测试的高质量模块。

**核心特点**：
1. **跨平台兼容**：支持大多数操作系统和编译器
2. **功能丰富**：包括智能指针、线程、正则表达式、文件系统等模块
3. **标准库兼容**：许多功能后来被纳入C++标准库
4. **高质量保证**：注重效率和跨平台兼容性

**应用领域**：
- **C++软件工程师**：高性能跨平台项目开发
- **系统架构师**：大型系统设计
- **学术研究者**：高性能数值计算、图算法、并行计算

Boost可视为C++的"工具箱"，提供标准库之外的实用工具，帮助开发者更快速、安全地完成项目。

---

## Socket网络通信

### 基本概念

Socket通信是计算机网络中的基础通信机制，允许不同设备、进程间交换数据。可以理解为程序间的"通信管道"。

### 应用场景

1. **即时通信**：微信、QQ、Slack等聊天工具
2. **Web通信**：HTTP/HTTPS底层基于TCP Socket
3. **网络游戏**：实时传输玩家位置、动作、状态
4. **文件传输**：FTP、SFTP等协议实现
5. **物联网设备**：智能家居设备与云服务器通信
6. **远程控制**：TeamViewer、SSH等工具
7. **音视频直播**：实时流媒体传输

### 游戏中的Socket应用

Socket在网络游戏中主要实现玩家间的实时同步。

**1. 位置同步流程**：
- 客户端获取角色新坐标`(x, y, z)`
- 通过Socket发送到游戏服务器
- 服务器更新世界状态并广播给其他玩家
- 其他客户端接收数据，更新角色位置显示

**2. 动作同步机制**：
- 客户端检测玩家动作（如"火球术"攻击）
- 发送动作事件：`{playerID: 123, action: "Fireball", target: 456}`
- 服务器验证动作合法性（冷却时间、命中判定等）
- 广播给相关玩家，显示技能效果和动画

**3. 状态同步管理**：
- 玩家A被玩家B攻击时
- 服务器计算伤害并更新玩家A血量
- 通过Socket通知相关玩家新的血量和状态
- 确保所有玩家看到一致的世界状态

### 技术要点

1. **实时性要求**：需要毫秒级更新，避免延迟、卡顿、瞬移
2. **数据优化**：传输增量数据而非完整状态，减少带宽占用
3. **协议选择**：
   - **TCP Socket**：可靠但有延迟，适用于聊天、交易等场景
   - **UDP Socket**：快速但不保证可靠，适用于位置、动作等实时更新
4. **客户端优化**：实现预测和插值算法，减少网络延迟影响

### 专业应用

- **网络游戏开发工程师**：设计客户端与服务器数据同步机制
- **后端游戏服务器工程师**：构建高效稳定的Socket服务
- **网络优化工程师**：降低延迟和丢包率，优化用户体验

---

## 实践验证方案

### 从原理到实战的升级路径

**目标设计**：
- 创建多人在线游戏原型（支持2-20个玩家）
- 模拟角色移动、攻击、状态同步
- 对比TCP/UDP在真实游戏环境下的表现

**系统架构**：
1. **服务器端**
   - 管理游戏状态（玩家位置、血量、动作）
   - 广播更新给所有客户端
   - 支持TCP和UDP模式切换

2. **客户端**
   - 发送本地玩家操作到服务器
   - 接收其他玩家数据，渲染游戏世界

3. **数据结构设计**
   - 位置包：`{playerID, x, y, z, timestamp}`
   - 动作包：`{playerID, action, targetID, timestamp}`
   - 状态包：`{playerID, HP, MP, statusFlags}`

### 验证方法

**实验设计**：
1. **UDP实时同步测试**
   - 客户端高频发送位置包（20-50ms间隔）
   - 服务器广播给所有玩家
   - 观察位置跳动和动作顺序问题

2. **TCP模式对比**
   - 使用TCP传输相同数据
   - 测量延迟变化和数据顺序保证

3. **负载测试**
   - 增加玩家数量模拟高负载
   - 测量网络延迟和丢包率

4. **客户端预测验证**
   - 为UDP模式添加预测和插值算法
   - 评估延迟对游戏体验的影响

### 技术实现要点

1. **数据压缩**：UDP传输增量数据，TCP可传输完整状态
2. **时间戳机制**：所有数据包含时间戳，便于判断顺序和丢包
3. **客户端平滑**：对UDP丢包进行插值或预测处理
4. **网络环境模拟**：使用工具增加延迟、丢包和抖动，测试各种网络条件

### 可视化验证

- 在客户端绘制角色轨迹线
- 丢包或乱序会表现为角色瞬移、动作错乱
- 对比TCP的轨迹平滑性和延迟表现

通过构建完整的多人在线原型，在TCP和UDP模式下测试玩家同步，可以最真实地观察到延迟、丢包和顺序问题，这是最接近大型游戏网络设计的验证方法。

---

**相关职业方向**：
- 网络游戏开发工程师
- 后端游戏服务器工程师  
- 实时系统工程师
- 游戏引擎开发者




## c++的底层原理在哪里学？

```markdown
## C++ 底层原理的学习路径

### 1. 语言标准与编译原理
- **C++ 标准文档**（ISO C++ Draft）  
- **《Programming Languages: C++》** by Bjarne Stroustrup  
- **编译原理 (Dragon Book)**  

内容：C++ 是如何从源代码变成机器码的。  

---

### 2. 编译器实现
- LLVM / Clang 源码  
- GCC 源码  
- 学习 C++ 的解析、AST（抽象语法树）、优化、代码生成。  

---

### 3. 运行时与内存模型
- **对象模型**：虚函数表、继承布局、对象内存布局。  
- **内存管理**：栈、堆、静态区。  
- 推荐：《Inside the C++ Object Model》（深度解析虚函数、对象布局）。  

---

### 4. 调试与反汇编
- 使用 **lldb** 或 **gdb**，结合 `-g` 和 `objdump -d`。  
- 学习 C++ 语句在汇编层面的对应。  

---

### 5. 系统接口
- **操作系统接口**：POSIX API、Windows API。  
- **C++ 与系统调用的关系**：I/O、线程、内存分配。  

---

## 推荐书籍
1. 《C++ Primer》 → 入门与实践  
2. 《Effective C++》 → 编程思想  
3. 《深入理解计算机系统 (CSAPP)》 → 底层运行机制  
4. 《Inside the C++ Object Model》 → 对象模型原理  

---

## 总结
学习 C++ 底层原理 = **语言标准 + 编译器 + 对象模型 + 汇编/系统接口**。  
常见路线：  
**C++ Primer → CSAPP → 编译原理 → LLVM → Inside C++ Object Model**  
```




## 问题，加搜索答疑

1. C++ 是 **静态类型语言**。这怎么理解？


## 如何学习 C++ 与系统接口

```markdown 
## 如何学习 C++ 与系统接口

### 1. 理解操作系统接口
- **POSIX API** → 类 Unix 系统的标准接口  
  - 文件操作：`open`, `read`, `write`, `close`  
  - 进程控制：`fork`, `exec`, `wait`  
  - 线程：`pthread_create`, `pthread_join`  
  - 网络：`socket`, `bind`, `listen`, `accept`  

- **Windows API** → Windows 系统特有接口  
  - 文件 I/O：`CreateFile`, `ReadFile`, `WriteFile`  
  - 进程：`CreateProcess`, `WaitForSingleObject`  
  - 线程：`CreateThread`  
  - 网络：Winsock（`WSAStartup`, `socket`）  

---

### 2. 在 C++ 中调用
- C++ 没有自己独立的系统调用，底层还是通过 **操作系统 API**。  
- 举例：`std::thread` 底层在 Linux 上调用 `pthread`，在 Windows 上调用 `CreateThread`。  

---

### 3. 学习方式
1. **书籍**  
   - 《Advanced Programming in the UNIX Environment》 (APUE)  
   - 《Windows System Programming》  

2. **实验**  
   - 在 Linux 用 `g++` 写一个文件读写程序，直接调用 `open`, `read`, `write`。  
   - 在 Windows 写一个小程序调用 `CreateFile` 和 `ReadFile`。  

3. **比较 C++ 封装与系统调用**  
   - 对比 `std::ifstream` vs `open/read`。  
   - 对比 `std::thread` vs `pthread_create`。  

---

### 4. 实践建议
- 从 **文件 I/O** 开始最直观，写简单 demo。  
- 然后学习 **进程 / 线程**，理解并发的实现方式。  
- 再深入 **网络编程**（socket）。  

---

## 总结
- C++ 的底层 I/O、线程等能力，本质上依赖操作系统接口。  
- 学习方法：读 API 文档 + 写小程序实验 + 对比 C++ 标准库封装。  
```

## C++ 是静态类型语言，这句话怎么理解？

```markdown  
C++ 是 **静态类型语言**。意思是：  
- 变量的类型在 **编译时** 就已经确定。  
- 编译器在编译阶段会进行类型检查。  
- 程序运行时不会改变变量的类型。  
```

## 代码分析

1. 虚函数和派生

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void speak() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void speak() override { cout << "Derived\n"; }
};

int main() {
    Base* p = new Derived();
    p->speak(); // 运行时决定调用 Derived::speak()
}
```
上面这段代码，里面怎么理解`Base* p = new Derived();`?

`Base* p = new Derived();` 体现了 **继承 + 多态** 的核心原理。  

`new Derived()`，这是在 堆内存 分配一块空间。同时调用 Derived 构造函数，构造出一个完整的 Derived 对象。返回一个指向该对象的 指针 (Derived*)。


## `using namespace std;` 的作用

```markdown
`using namespace std;` 的作用是把 **命名空间 `std`** 下的所有名字引入当前作用域。  

在 C++ 中，标准库的大部分内容（如 `cout`, `cin`, `string`, `vector`）都定义在 `namespace std` 里面。  
加上 `using namespace std;` 后，可以直接写 `cout << ...`，而不用写 `std::cout << ...`。
```

## xxx.h文件

xxx.h文件和xxx.cpp文件一般都是同时出现的，我的理解是，有时候xxx.cpp是闭源的，这时xxx.h就相当于一个开放的api。

## C++ 注释写法

```cpp
// 单行注释
/* 多行注释 */
```

## include的两种写法

```cpp
#include <stdio.h>
#include "lib/lowlevelmath.h"
```

## bash环境中gcc的用法

```bash
# 在 **GCC** 和 **Clang** 中，命令行选项的顺序基本没有区别。  
- `gcc -c -o main.o code.c`  
- `clang -c -o main.o code.c`  
```

## 头文件不用编译

在 C/C++ 项目里：
- `.h` / `.hpp` 文件是 **头文件**，只包含声明（函数原型、类定义、宏、常量等），通常不单独编译。  
- `.c` / `.cpp` 文件是 **源文件**，包含具体实现，需要编译生成 `.o` 或 `.obj`。  
- 编译器在编译 `.cpp` 时，会把 `#include "xxx.h"` 展开，相当于把头文件内容复制进来。  

## 为什么 `xxx.cpp` 要 `#include "xxx.hpp"`?

核心原因：**保持声明与实现一致**。但是我没学明白这里。

## 基本上所有的hpp文件里面都要写#ifndef

目的是：防止头文件被多次包含，导致重复定义错误。








## 我整理的自用学习资源

1. 学习 c/cpp 的头文件原理。[c header](https://www.youtube.com/watch?v=tOQZlD-0Scc).
2. cpp的创始人，回答学会cpp要多久的视频。[cpp how long to learn](https://www.youtube.com/watch?v=oIFkg1zQE-0).
3. 

