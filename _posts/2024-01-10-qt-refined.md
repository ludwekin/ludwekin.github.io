---
title: "QT学习笔记"


---

## QT的使用场景

Qt 是一个跨平台的应用开发框架，主要用于 GUI（图形用户界面）开发，但也可以用于非 GUI 应用的开发（如控制台工具、网络服务等）。它的应用场景非常广泛：

1. **嵌入式设备**  
   - 在 ARM 芯片开发板上加屏幕后，很多图形界面都是使用 Qt 开发的。  
   - 常见设备包括智能家居控制面板、工业控制终端、医疗设备等。

2. **汽车电子**  
   - 中控屏的图形界面、仪表盘界面等。  
   - 支持 Qt Quick/QML 动画和交互，方便快速迭代 UI。

3. **PC应用程序**  
   - 桌面软件、图像处理工具、数据分析应用等。

4. **跨平台应用**  
   - Windows、macOS、Linux、嵌入式系统都可支持。  
   - 使用 Qt 可以避免重复开发不同平台的 GUI。

---

## 从模仿到创造


### 阶段一：模仿 —— 从官方示例开始

- 使用 Qt Creator 打开官方示例，熟悉项目结构。  
- 理解 `.h`（声明）与 `.cpp`（实现）文件的分工。  
- 熟悉 **信号与槽（Signal/Slot）**，掌握按钮与界面交互。

### 阶段二：改造 —— 扩展示例功能

- 在原有示例基础上增加功能模块。  
- 尝试调整界面布局，让窗口更美观。  
- 学习 Qt Designer 或 QML 创建界面，并与 C++ 逻辑结合。

### 阶段三：创造 —— 从零开发小项目

- 根据个人需求设计现代化界面。  
- 分层管理数据、逻辑与界面。  
- 使用 CMake 管理项目，学习资源系统（`.qrc` 文件）。

---

## QT知识点杂碎汇总

1. **Qt Quick Column**  
   - `Column` 是 QML 布局容器，将子元素垂直排列。  

2. **Qt Design Studio**  
   - 专业 UI 设计工具，用于快速创建和编辑 Qt Quick/QML 界面。  
   - 可视化拖拽，不必手写 QML。

3. **Figma**  
   - 核心用户为 UI/UX 设计师。  
   - 前端和客户端工程师也常用，用于设计 App、网站、桌面软件界面。

4. **Qt Maintenance Tool**  
   - 管理 Qt 安装、更新和组件增加/卸载。

5. **CMake 与 Ninja**  
   - CMake：跨平台构建系统生成器，不直接编译代码。  
   - Ninja：快速、高效的编译工具，常与 CMake 配合使用。  
   - 历史：CMake 诞生于 2000 年，解决大型跨平台项目构建问题。Ninja 由 Google 工程师 Evan Martin 为 Chromium 项目设计。

6. **Make 与 Makefile**  
   - Makefile：描述编译规则文件，配合 Make 工具使用。  
   - Make 工具由贝尔实验室 Feldman 发明。

7. **POSIX**  
   - Portable Operating System Interface，由 IEEE 制定。  
   - 定义操作系统接口标准，方便跨平台开发。

8. **Qt Widget 与 MainWindow**  
   - `QWidget`：通用界面元素基类。  
   - `QMainWindow`：专为主窗口设计，带完整框架（菜单栏、状态栏、工具栏）。

9. **头文件与实现文件**  
   - `.h`：声明接口与成员。  
   - `.cpp`：实现功能和逻辑。

10. **PySide6**  
    - Python 绑定 Qt，用 Python 开发 Qt 应用。

11. **ApplicationFlow（Qt Design Studio）**  
    - 将应用看作一系列页面（Pages/Screens）。  
    - 页面通过状态机或流转关系连接，可视化定义跳转逻辑。  
    - 工具生成对应 QML/代码执行流程。

12. **没有 Qt 时的 C++ GUI 开发（Windows）**  
    - Win32 API：基础接口（User32.dll, GDI32.dll 等）。  
    - MFC：Win32 封装的 C++ 类库。  
    - 跨平台 GUI 库：封装 Win32 API，简化开发。

13. **QML 与 .ui.qml**  
    - `.qml`：声明式 UI 文件，可写 JS 处理逻辑。  
    - `.ui.qml`：纯 UI 文件，由 Qt Design Studio 生成，不写逻辑。

14. **Qt 与 C++ 的关系**  
    - 前后端分离模式：QML 负责界面，C++ 处理核心逻辑。  
    - QML：快速搭建界面，可写轻量交互逻辑。  
    - C++：处理算法、数据库、网络、多线程等。

15. **MinGW32 编译器**  
    - Minimalist GNU for Windows。  
    - 用于生成原生 Windows 可执行文件（.exe/.dll）。

16. **栈与堆**  

| 内存类型 | 特点 | 适用场景 | 示例 |
|----------|------|----------|------|
| 栈（stack） | 编译器自动管理，函数结束自动释放 | 临时对象 | `QWidget w;` |
| 堆（heap） | 程序员手动管理，生命周期可控 | 长期存在对象 | `QWidget *w = new QWidget();` |

17. **Qt 元对象系统**  
    - 扩展 C++ 功能，实现信号/槽、运行时类型信息、动态属性。  
    - 依赖 `Q_OBJECT` 宏和 moc 编译阶段生成的代码。

18. **Qt 控件父子关系**  
    - 父控件管理子控件位置、大小和生命周期。  
    - 例：`QPushButton` 的父控件是 `QMainWindow`。

19. **tr()**  
    - 国际化翻译函数，将文本标记为可翻译。  
    - 配合 Qt 翻译工具生成 `.qm` 文件。

20. **C++ const 与 static 区别**  

| 特性 | const | static |
|------|-------|--------|
| 修饰变量 | 值不可修改 | 生命周期延长至程序结束或作用域受限 |
| 修饰函数参数 | 参数不可修改 | 不适用 |
| 修饰成员函数 | 不修改对象成员 | 表示类函数（不依赖对象） |
| 类的成员变量 | 每个对象一份 | 所有对象共享 |
| 文件作用域 | ❌ | 限定在当前 `.cpp` 文件可见 |

---

## hands-on 示例

### 点击按钮选择图片并显示

```cpp
#include <QApplication>
#include <QWidget>
#include <QPushButton>
#include <QFileDialog>
#include <QLabel>
#include <QVBoxLayout>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    QWidget window;
    window.setWindowTitle("Qt 示例");

    QVBoxLayout *layout = new QVBoxLayout(&window);

    QLabel *label = new QLabel("未选择图片");
    QPushButton *button = new QPushButton("选择图片");

    layout->addWidget(label);
    layout->addWidget(button);

    QObject::connect(button, &QPushButton::clicked, [&](){
        QString fileName = QFileDialog::getOpenFileName(&window, "选择图片", "", "Images (*.png *.jpg *.bmp)");
        if (!fileName.isEmpty()) {
            label->setText(fileName);
        }
    });

    window.show();
    return app.exec();
}
```