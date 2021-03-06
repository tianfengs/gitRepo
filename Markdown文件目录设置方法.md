# 如何为Markdown文件生成目录

[1. 手动生成](#手动生成)

​	[- 方法一](#方法一)

​	[- 方法二](#方法二)

[2. 自动生成](#自动生成)

​	[-  使用Visual Studio Code 的 TOC扩展](#使用Visual Studio Code 的 TOC扩展)

​	[- Pandoc 命令](#Pandoc 命令)



1. ## 手动生成
   - 方法一：
     - 设置目录

       [A successful Git branching model笔记](#1)

   - 设置目标

     ​	<a name="1"/>1.1. Feature分支

   - 方法二：（推荐用这个方法）
     - 设置目录

       [目录表中的item1][#目录表中的item1]

     - 设置目标    

       目录表中的item1

2. ## 自动生成
   - 使用Visual Studio Code 的 TOC扩展

     - **单击 VS Code 的扩展图标，在搜索框搜索 Markdown TOC 并安装。**
     - **点击菜单栏的文件 -> 打开，打开需要生成目录的 .md 格式的文件。**
     - **将光标移至需要插入目录的位置，右键单击 Markdown TOC: Insert/Update [^M T]，目录即自动插入。**
     - *单击右上角的预览图标，可查看目录的显示效果。
     - 保存，关闭文件**

   - Pandoc 命令。

     Pandoc 是由 John MacFarlane 开发的标记语言转换工具，可实现不同标记语言间的格式转换，堪称该领域中的“瑞士军刀”。Pandoc 使用 Haskell 语言编写，以命令行形式实现与用户的交互，可支持多种操作系统。  如果你想了解 pandoc 的更多功能和参数使用，可参考 pandoc 官网的文档：[https://pandoc.org/MANUAL.html](https://link.jianshu.com/?t=https://pandoc.org/MANUAL.html)

     - **下载和安装 pandoc。**
     - *打开终端，输入 pandoc --version 确认 pandoc 已成功安装。
     - 依次使用以下命令，转到 FAQ.md 文件所在的目录。**（详细的使用说明可参阅：[https://pandoc.org/getting-started.html#step-3-changing-directories](https://link.jianshu.com/?t=https://pandoc.org/getting-started.html#step-3-changing-directories)）
       - `pwd`：查看查看当前目录路径，“printing working directory” 的缩写。
       - `ls`：查看目录列表，“list segment” 的缩写。
       - `cd`：转到某目录下，是 “change directory” 的缩写。
     - **输入以下命令，即可自动生成目录**

   ```
   pandoc -s --toc --toc-depth=4 FAQ.md -o FAQ.md
   注：pandoc 默认生成三级目录。以上述命令为例，如果使用如下命令则只会生成三级目录：
   pandoc -s --toc FAQ.md -o FAQ.md
   ```