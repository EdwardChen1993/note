[TOC]

# git



## 分支

### 删除远程分支

```bash
git push origin --delete 分支名
# 或者
git push origin :分支名
```



## 标签

### 推送远程标签

**推送特定标签：**

```bash
git push origin 标签名
```

**推送所有标签：**

```bash
git push origin --tags
```



### 删除远程标签

```bash
git push origin :ref/tags/标签名
```



## 生成.gitignore文件

### 方法一

使用 `github/gitignore`上面对应的文件：[链接](https://github.com/github/gitignore)



### 方法二

搜索操作系统, IDEs, 或编程语言：[链接](https://www.toptal.com/developers/gitignore)



### 方法三

vscode使用 `.gitignore Generator` 插件



## .gitignore规则不生效解决方法

`.gitignore` 只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改 `.gitignore` 是无效的。

解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

```bash
git rm -rf --cached .
git add .
git commit -m 'update .gitignore'
```



## git reset

### 三种模式区别

**--hard：重置stage区和工作目录**

`--hard` 会在重置 HEAD 和branch的同时，重置stage区和工作目录里的内容。当你在 reset 后面加了` --hard` 参数时，你的stage区和工作目录里的内容会被完全重置为和HEAD的新位置相同的内容。换句话说，就是你的没有commit的修改会被全部擦掉。



**--soft：保留工作目录，并把重置 HEAD 所带来的新的差异放进暂存区**

`--soft` 会在重置 HEAD 和 branch 时，保留工作目录和暂存区中的内容，并把重置 HEAD 所带来的新的差异放进暂存区。



**不加参数(--mixed)：保留工作目录，并清空暂存区**

如果不加参数，那么默认使用 `--mixed` 参数。它的行为是：保留工作目录，并且清空暂存区。也就是说，工作目录的修改、暂存区的内容以及由 reset 所导致的新的文件差异，都会被放进工作目录。简而言之，就是「把所有差异都混合（mixed）放在工作目录中」。



## git clone

```bash
git clone 仓库地址
```

参数：

-b：指定分支名

--depth：指定只克隆最近提交的几个版本，可以加快克隆速度。