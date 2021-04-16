[TOC]

# git

## 删除远程分支

```bash
git push origin --delete 分支名
# 或者
git push origin :分支名
```



## 推送远程标签

推送特定标签：

```bash
git push origin 标签名
```

推送所有标签：

```bash
git push origin --tags
```



## 删除远程标签

```bash
git push origin :ref/tags/标签名
```





## 生成.gitignore文件

### 方法一

使用github/gitignore上面对应的文件：[链接](https://github.com/github/gitignore)



### 方法二

搜索操作系统, IDEs, 或编程语言：[链接](https://www.toptal.com/developers/gitignore)



### 方法三

vscode使用.gitignore Generator插件



## .gitignore规则不生效

.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。

解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

```bash
git rm -rf --cached .
git add .
git commit -m 'update .gitignore'
```

