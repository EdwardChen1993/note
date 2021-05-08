[TOC]

# NPM node包管理工具



## 发布一个新包



### 第一步、进入要发布的项目根目录，初始化为npm包

依次按提示填入包名、版本、描述、github地址、关键字、license等

```bash
npm init
```

这步完成之后会生成一个package.json文件，上面输入的这些信息可以在该文件中修改



### 第二步、注册npm用户

方法一、npm官网注册：[npm](https://www.npmjs.com/)

方法二、使用npm命令注册（注册成功后自动登录）：

```bash
npm adduser
```



### 第三步、账号登录

如果使用第二步中的方法一，需要输入账号、密码、邮箱登录。使用方法二则不需要。

```bash
npm login
```



### 第四步、发布包，上传到npm包服务器

```bash
npm publish
```

“+”符号表示发布成功，可以去自己的npm主页上验证一下，可以看到发布的新包已经在列表中。

**注意：如果发布时报错：‘no_perms Private mode enable, only admin can publish this module:’，表示当前不是原始镜像，可能用的是其他镜像，如淘宝镜像。**



要切换回原始的npm镜像，命令：

```bash
npm config set registry https://registry.npmjs.org
```



## 更新一个已经发布的包



### 第一步、修改包的版本

```bash
npm version patch
```

该命令在原来的版本上自动加1，实际上是将package.json文件中的version值修改了。



### 第二步、重新发布包

```bash
npm publish
```



## 删除包



### 删除指定版本

```bash
npm unpublish 包名@版本号
```



### 删除整个包

```bash
npm unpublish 包名 --force
```





## npm-check-updates

npm包高效升级插件。

安装：

```bash
npm install -g npm-check-updates
```



显示当前目录下项目的任何新依赖项:

```bash
ncu
```



检查全局的包：

```bash
ncu -g
```



升级项目的包文件:

```bash
ncu -u
```

注意：上述命令只是更新package.json文件的内容，并没有修改node_modules目录下的依赖包。所以需要执行以下命令更新node_modules目录下的依赖包：

```bash
rm -rf node_modules/
npm install
```





## 语义化版本

### 示例

1.2.3-beta.1-meta

1——>主版本

2——>次版本

3——>修订版本

beta——>先行版本

1——>先行版本的版本号

meta——>元数据，对版本号做简要描述



### 版本名称释义

| 名称    | 释义                                                         |
| ------- | ------------------------------------------------------------ |
| alpha   | 内部测试版本，除非是内部测试人员，否则不推荐使用，有很多bug。 |
| beta    | 公测版本，消除了严重错误，还是会有缺陷，这个阶段还会持续加入新的功能。 |
| rc      | Release Candidate，发行候选版本。这个版本不会加入新的功能，主要排错，修改bug。 |
| release | 发行版本。                                                   |



## npm-run-all

并行或顺序运行多个npm脚本的CLI工具



### 安装

```bash
npm install npm-run-all --save-dev
```



### 用法

```bash
# 串行执行
npm-run-all -s 脚本1 脚本2 脚本3 
# 并行执行
npm-run-all -p 脚本1 脚本2 脚本3 
```

