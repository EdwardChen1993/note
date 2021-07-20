[TOC]

# npm

node包管理工具



## dependencies、devDependencies、peerDependencies

### dependencies和devDependencies

当使用 `npm install <package-name>` 安装 npm 软件包时，是将其安装为依赖项。

该软件包会被自动地列出在 package.json 文件中的 `dependencies` 列表下（在 npm 5 之前：必须手动指定 `-S` 或 `--save`）。

当添加了 `-D` 或 `--save-dev` 标志时，则会将其安装为开发依赖项（会被添加到 `devDependencies` 列表）。

开发依赖是仅用于开发的程序包，在生产环境中并不需要。 例如**测试的软件包、webpack 或 Babel**。

当投入生产环境时，如果输入 `npm install` 且该文件夹包含 `package.json` 文件时，则会安装它们，因为 npm 会假定这是开发部署。

需要设置 `--production` 标志（`npm install --production`），以避免安装这些开发依赖项。

### peerDependencies

假设我们当前的项目是MyProject，项目中有一些依赖，比方其中有一个依赖包**PackageA**，该包的**package.json**文件指定了对**PackageB**的依赖：

```json
{
    "dependencies": {
        "PackageB": "1.0.0"
    }
}
```

如果我们在我们的MyProject项目中执行`npm install PackageA`, 我们会发现我们项目的目录结构会是如下形式：

```bash
MyProject
|- node_modules
   |- PackageA
      |- node_modules
         |- PackageB
```

那么在我们的项目中，我们能通过下面语句引入**PackageA**：

```js
var packageA = require('PackageA')
```

但是，如果你想在项目中直接引用PackageB:

```js
var packageA = require('PackageA')
var packageB = require('PackageB')
```

这是不行的，即使**PackageB**被安装过；因为Node只会在 `MyProject/node_modules`目录下查找**PackageB**，它不会在进入**PackageA**模块下的 `node_modules` 下查找。

所以，为了解决这个问题，在MyProject项目`package.json`中我们必须直接声明对**PackageB**的依赖并安装。

但是，有时我们不用在当前项目中声明对**PackageB**的依赖就可以直接引用，尤其是，**PackageA**是一个类似于**grunt**的插件，例如`grunt-contrib-jshint`。

为什么在项目中不用声明就可以直接使用呢？这就不得不说说**peerDependencies**的作用了。

为了解决这种问题：如果你安装我，那么你最好也安装X,Y和Z.

于是`peerDependencies`就被引入了。例如上面**PackageA**的`package.json`文件如果是下面这样：

```json
{
    "peerDependencies": {
        "PackageB": "1.0.0"
    }
}
```

那么，它会告诉npm：如果某个package把我列为依赖的话，那么那个package也必需应该有对**PackageB**的依赖。

也就是说，如果你`npm install PackageA`，你将会得到下面的如下的目录结构：

```bash
MyProject
|- node_modules
   |- PackageA
   |- PackageB
```

下面的代码现在可以正常工作了，因为两个包在`MyProject/node_modules`中被安装了：

```javascript
var packageA = require('PackageA')
var packageB = require('PackageB')
```

总结一句话，**peerDependencies**的具体作用：

> **peerDependencies**的目的是提示宿主环境去安装满足插件**peerDependencies**所指定依赖的包，然后在插件`import`或者`require`所依赖的包的时候，永远都是引用宿主环境统一安装的npm包，最终解决插件与所依赖包不一致的问题。

举个例子，就拿目前基于react的ui组件库``来说，因该ui组件库只是提供一套react组件库，它要求宿主环境需要安装指定的react版本。具体可以看它 [package.json](https://github.com/ant-design/ant-design/blob/master/package.json#L37) 中的配置：

```json
  "peerDependencies": {
    "react": ">=16.0.0",
    "react-dom": ">=16.0.0"
  }
```

它要求宿主环境安装`react@>=16.0.0`和`react-dom@>=16.0.0`的版本，而在每个antd组件的定义文件顶部：

```javascript
import * as React from 'react';
import * as ReactDOM from 'react-dom';
```

组件中引入的react和react-dom包其实都是宿主环境提供的依赖包。



## 发布包

### 第一步、进入要发布的项目根目录，初始化为npm包

依次按提示填入包名、版本、描述、github地址、关键字、license等

```bash
npm init
```

这步完成之后会生成一个 `package.json` 文件，上面输入的这些信息可以在该文件中修改



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



## 更新包

### 第一步、修改包的版本

```bash
npm version patch
```

该命令在原来的版本上自动加1，实际上是将 `package.json` 文件中的version值修改了。



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

注意：上述命令只是更新 `package.json` 文件的内容，并没有修改 `node_modules` 目录下的依赖包。所以需要执行以下命令更新 `node_modules` 目录下的依赖包：

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

