[TOC]

# NVM node版本管理工具



## 安装

windows：

[链接](https://github.com/coreybutler/nvm-windows/releasesd)

macos：

[链接](https://github.com/nvm-sh/nvm)



## 使用



### 安装多版本node

```bash
nvm install 版本号
```



### 列出本地已安装实例

```bash
nvm ls
```



### 列出远程服务器上所有的可用版本

macos：

```bash
nvm ls-remote
```

windows：

```bash
nvm ls available
```



### 在不同版本间切换

切换到具体版本：

```bash
nvm use 版本号
```

切换到最新版本：

```bash
nvm use node
```

