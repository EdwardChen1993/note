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



## 关闭程序端口占用

windows：

**占用查询端口的pid查询：**

```bash
netstat -ano | findstr 端口号
```

![image-20210508113825176](node.assets/image-20210508113825176.png)



**关闭对应pid：**

```bash
taskkill -F -PID 对应的PID
```

![image-20210508113926552](node.assets/image-20210508113926552.png)

## 