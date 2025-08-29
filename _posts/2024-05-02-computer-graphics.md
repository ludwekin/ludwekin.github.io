---
title : "计算机图形学，相关知识点及个人理解"

---

1. Order Independent Transparency 是什么？

    - 这个话题，属于 计算机图形学 (Computer Graphics) 领域。
    - OIT 就是 不需要提前对透明物体排序，而是利用一些算法和 GPU 技术，让渲染结果自动正确。
2. WebGL vs OpenGL 区别。
    - WebGL
	    - 浏览器中运行（JavaScript API）
	    - 基于 OpenGL ES 2.0/3.0 规范
	    - 无需安装驱动，靠浏览器和 GPU 驱动适配
        - 在 HTML5 <canvas> 中渲染
        - 功能受限，只提供 OpenGL ES 的子集
        - 适合网页 3D 展示、Web 游戏
    - OpenGL
		- 桌面端/本地应用使用
	    - Windows、Linux、macOS 上运行
	    - 需要 GPU 驱动支持
        - 通过函数调用 GPU（例如 glDrawArrays）
        - 功能更完整，支持高级特性（Tessellation、Compute Shader、Geometry Shader 等）
        - 适合大型 3D 游戏/工业图形应用