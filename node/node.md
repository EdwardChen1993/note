[TOC]

# node



## __dirname和process.cwd()的区别

process.cwd() 返回当前工作目录。如：调用node命令执行脚本时的目录。
__dirname 返回源代码所在的目录。

例如：对于d:\dir\index.js。

| **命令**          | **process.cwd()** | **__dirname** |
| ----------------- | ----------------- | ------------- |
| node index.js     | d:\dir            | d:\dir        |
| node dir\index.js | d:                | d:\dir        |

