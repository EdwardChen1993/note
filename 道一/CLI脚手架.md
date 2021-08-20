[TOC]

# CLI脚手架

## 概述

很多小伙伴一直很纠结什么是脚手架，其实核心功能就是创建项目初始文件。那么问题来了，市面上的脚手架如`vue-cli` 、`create-react-app`、`dva-cli`这一类的官方脚手架不是够用吗，为什么需要自己写？

归根到底，还是因为他们的特点，他们的特点不用多说那就是**专一**。 但是专一是优点的同时，其实也有可能是缺点。

此外在公司开发过程中你会发现有以下一系列的问题!

- 业务类型多
- 多次造轮子，项目升级等问题
- 公司代码规范，无法统一

很多时候我们开发时需要新建项目，把已有的项目代码复制一遍，保留基础能力（但是这个过程非常琐碎而又耗时）。那我们可以自己定制化模板，自己实现一个属于自己的脚手架，来解决以上的这些问题。



## 官方示例

在自己开发 CLI 脚手架之前，那肯定要先看下市面上优秀的脚手架是什么样子的。

安装`vue-cli`：

```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

使用`vue-cli`：

```bash
vue create [project-name]
```



## 前置知识

### npm link

npm link 用来在本地项目和本地开发的 npm 模块之间建立**软连接**（相当于快捷方式），可以在本地进行模块测试，而不需要频繁发布模块到远程 npm 上。

具体用法：

**1. 项目和模块在同一个目录下，可以使用相对路径**

```bash
npm link ../module
```

**2. 项目和模块不在同一个目录下**

cd 到模块目录，`npm link`，进行全局link

cd 到项目目录，`npm link 模块名`

**3. 解除link**

解除模块全局link，模块目录下，`npm unlink 模块名`

解除项目和模块link，项目目录下，`npm unlink 模块名`



### bin 字段

在 `package.json` 中可以指定一个 `bin` 字段，它告诉 npm，里面指定的 js 脚本可以通过命令行的方式执行，以 `do1-cli` 的命令调用。当然命令行的名字你想写什么都是你的自由，比如：

指定字符串形式，命令则默认以 `name` 的属性值进行调用，即 `do1-cli`：

```json
{
  "name": "do1-cli",
  "bin": "./bin/index.js"
}
```

指定对象形式，命令则以对象的键名进行调用，即 `do1`：

```json
{
  "bin": {
    "do1": "./bin/index.js"
  }
}
```

同时，需要在`bin`字段指定的 js 脚本头部指定`#!/usr/bin/env node`，这是告诉系统，以下的脚本内容，需要使用 node 来执行（类似  shell脚本头部的 `#!/bin/bash` ）。



## 实现思路

1. 配置可执行命令 **commander** 

   [官方文档](https://github.com/tj/commander.js/blob/master/Readme_zh-CN.md)

2. 命令行交互功能 **inquirer**

   [官方文档](https://www.npmjs.com/package/inquirer#documentation)

3. 下载模板 **download-git-repo**

   [官方文档](https://www.npmjs.com/package/download-git-repo)



## 具体实现

### 设想

在开始实现 CLI 脚手架之前，我们设想一下如何初始化项目的命令：

```bash
do1 create <project-namne>
```



### 创建工程

1. 初始化 npm 

   ```bash
   npm init -y
   ```

2. 创建工程目录

   ```bash
   ├── bin
   │   └── index  		// 全局命令执行的脚本
   ├── lib
   │   ├── create.js // 接收对应的命令后具体实现函数，一个命令对应个 js 文件，create 命令
   │   ├── config.js // config 命令
   │   └── ui.js 		// ui 命令
   │── package.json
   ```

3. 链接全局包

   设置在命令下执行 `do1` 时调用 `bin` 目录下的 `index.js` 文件

   ```bash
   {
     "bin": {
       "do1": "./bin/index.js"
     }
   }
   ```

   并且指定 `bin` 目录下的 `index.js` 文件以 node 环境执行

   ```js
   #!/usr/bin/env node
   ```

   

### 配置可执行命令

commander 是完整的 node 命令行解决方案。比如 `vue-cli`的命令都是使用 commander 实现的：

```bash
vue-cli --version
vue-cli --help
vue-cli create <project-namne>
```



#### 安装

```bash
npm install commander
```



#### 使用

bin/index.js：

首先指定版本号，这样就可以使用`do1 -V` 或者 `do1 --version` 输出版本号。

```js
const program = require('commander');

program
  .version('0.0.1')
  .parse(process.argv); // 解析用户执行命令传入的参数，process.argv 就是用户在命令行中传入的参数
```

此时这个版本号应该使用的是当前项目的版本号，所以我们需要动态获取，因为 `package.json` 文件已经记录了项目的版本号，所以我们直接引入 `package.json` 即可：

```js
const program = require('commander');
const pkg = require("../package.json");

program
  .name(pkg.name)
  .version(pkg.version)
  .parse(process.argv); // 解析用户执行命令传入的参数，process.argv 就是用户在命令行中传入的参数
```



####  配置指令命令

```js
program
  .name('do1')
  .version(pkg.version)
  .usage("<command> options")
  .command("create <app-name>")
  .description("create a new project")
  .option("-f, --force", "overwrite target directory if it exits")
  // 执行上面 command 命令的这个行为时，触发回调
  .action((name, cmd) => {
    // 调用 create 模块实现
    require("../lib/create")(name, cmd);
  });
```



#### 监听 --help 命令

监听 `--help` 命令打印帮助信息，模仿 `vue-cli` 的输出样式：

```js
const program = require("commander");
const chalk = require("chalk");

program.on("--help", () => {
  console.log(
    `\nRun ${chalk.blue(
      "do1 <command> --help"
    )} for detailed usage of given command.\n`
  );
});
```

到现在我们已经基本把命令行配置齐全了，接下来就开始实现输入命令对应的功能。



## create 命令

create命令的主要作用就是去 git 仓库中拉取模板并下载对应的版本到本地目录，从而实现初始化项目。



### 创建命令对应文件

创建 `create.js` ：

```js
module.exports = async function (projectName, options) {
  
}
```



### 拉取项目

我们需要从 GitHub 中的所有模板信息，用到的模板已经事先放到 [GitHub ](https://github.com/EdwardChen1993?tab=repositories) 上面去了。

你可以使用 GitHub 的 API （https://api.github.com）来创建调用来获取与 GitHub 集成所需的数据。其中可以获取仓库数据、分支数据、标签数据等。

在脚手架中，我们需要用到两个 API 接口，分别是：

`https://api.github.com/users/${USER}/repos`

`https://api.github.com/repos/${USER}/${repo}/tags`

此外，要从服务端调用 API 接口获取数据，我们需要使用 `axios` ：

```bash
npm i axios
```

lib/request.js

```js
const axios = require("axios");
const ora = require("ora");
const loading = ora("fetching");

// 请求拦截器
axios.interceptors.request.use((config) => {
  loading.start();
  return config;
});
// 响应拦截器
axios.interceptors.response.use(
  (res) => {
    loading.succeed('fetch completed');
    return res.data;
  },
  (error) => {
    loading.fail("fetch fail, please try again")
    return Promise.reject(error)
  }
);

// 获取用户 github 上的仓库列表
async function fetchRepoList() {
  return axios.get("https://api.github.com/users/EdwardChen1993/repos");
}

// 获取用户某个仓库列表下的标签列表
async function fetchTagList(repo) {
  return axios.get(`https://api.github.com/repos/EdwardChen1993/${repo}/tags`);
}

module.exports = { fetchRepoList,fetchTagList };
```



### 询问交互

我们需要通过命令行询问交互的方式来获取到用户需要使用的模板之类的信息，所以就需要使用到 `inquirer`

```bash
npm i inquirer
```

`lib/create.js`

 ```js
 const fs = require("fs-extra");
 const path = require("path");
 const inquirer = require("inquirer");
 const Creator = require('./Creator')
 
 module.exports = async function (projectName, options) {
   // 获取当前命令执行时的工作目录
   const cwd = process.cwd();
   const targetDir = path.join(cwd, projectName);
   // 判断是否存在项目名的目录
   if (fs.existsSync(targetDir)) {
     // 强制创建，删除存在的
     if (options.force) {
       await fs.remove(targetDir);
     }
     // 询问用户确认是否覆盖
     else {
       const { action } = await inquirer.prompt([
         {
           name: "action",
           type: "list",
           message: "Target directory already exists pink an action:",
           choices: [
             {
               name: "Overwrite",
               value: "Overwrite",
             },
             {
               name: "Merge",
               value: "Merge",
             },
             {
               name: "Cancel",
               value: false,
             },
           ],
         },
       ]);
       // 取消操作
       if (!action) {
         return;
       } 
       // 重写操作
       else if (action === "Overwrite") {
         await fs.remove(targetDir);
       }
       // 合并操作
       else if (action === "Merge") {
       }
     }
   }
 
   // 开始创建项目
   const creator = new Creator(projectName, targetDir)
   creator.create()
 };
 
 ```



### 获取仓库和标签数据，然后下载模板

从 GitHub 或 GitLab 中下载模板，需要用到 `download-git-repo`

```bash
npm i download-git-repo
```

注意该模块导出的不支持 `promise`，需要我们使用 node 中已经帮你提供了一个现成的方法 `promisify`，将异步的api可以快速转化成promise的形式

```js
const { promisify } = require("util");
const downloadGitRepo = require("download-git-repo");
promisify(downloadGitRepo);
```



`lib/Creator.js`

```js
const inquirer = require("inquirer");
const { promisify } = require("util");
const path = require("path");
const downloadGitRepo = require("download-git-repo");
const chalk = require("chalk");
const ora = require("ora");
const loading = ora("fetching");
const { fetchRepoList, fetchTagList } = require("./request");

class Creator {
  constructor(projectName, targetDir) {
    this.name = projectName;
    this.target = targetDir;
    this.downloadGitRepo = promisify(downloadGitRepo);
  }

  async fetchRepo() {
    let repos = await fetchRepoList();
    if (!repos) return;
    repos = repos
      .filter((item) => ["vue", "react"].includes(item.name)) // 为了演示过滤掉和模板无关的仓库
      .map((item) => item.name);
    let { repo } = await inquirer.prompt({
      name: "repo",
      type: "list",
      choices: repos,
      message: "please choose a framework to create your project",
    });
    return repo;
  }

  async fetchTag(repo) {
    let tags = await fetchTagList(repo);
    if (!tags) return;
    tags = tags.map((item) => item.name);
    let { tag } = await inquirer.prompt({
      name: "tag",
      type: "list",
      choices: tags,
      message: "please choose a version to create your project",
    });
    return tag;
  }

  async fetchIsUseTS() {
    const { useTS } = await inquirer.prompt({
      name: "useTS",
      type: "confirm",
      message: "please confirm whether to use typescript",
      default: false,
    });
    return useTS;
  }

  async download(repo, tag, isUseTS) {
    // 1.拼接下载路径
    // 可以直接使用简写路径
    // GitHub - github:owner/name or simply owner/name
    let requestUrl;
    // 使用 typescript ，下载路径使用分支名，否则使用标签名
    if (isUseTS) {
      const branch = `${repo}${tag.split(".")[0]}-template-with-typescript`;
      requestUrl = `EdwardChen11993/${repo}#${branch}`;
    } else {
      requestUrl = `EdwardChen1993/${repo}#${tag}`;
    }
    // 2.将资源下载到某个目录下
    loading.start();
    try {
      await this.downloadGitRepo(
        requestUrl,
        path.resolve(process.cwd(), this.target)
      );
      loading.succeed("download completed");
    } catch (error) {
      loading.fail("download fail, please try again");
      return Promise.reject(error);
    }
    return this.target;
  }

  async create() {
    // 采用远程拉取的方式 -> github
    // 1.拉取当前账号下的模板
    const repo = await this.fetchRepo();

    // 2.通过模板查找版本号
    const tag = await this.fetchTag(repo);

    // 3.询问是否使用 typescript
    const isUseTS = await this.fetchIsUseTS();

    // 4.下载
    await this.download(repo, tag, isUseTS);

    // 5.处理下载完成后逻辑，提示用户接下来的操作
    console.log(`🎉  Successfully created project ${chalk.yellow(this.name)}`);
    console.log("👉  Get started with the following commands:");
    console.log(chalk.blue(`$ cd ${this.name}`));
    console.log(chalk.blue("$ npm i"));
    console.log(chalk.blue("$ npm run serve"));
  }
}

module.exports = Creator;
```



## 项目发布

当我们的模块开发完成之后，就可以发布到远程 npm 上供其他人使用，**发布前可以使用 `npm unlink` 解除此前开发时设置的链接全局软连接。**

```bash
npm unlink
```

发布前先检查自己是否已经登录了 npm 账号，**注意检查登录态前必须要将镜像源设置为官方npm的而不是淘宝的，否则无法检查登录态和登录**

```bash
npm config get registry # 查看设置的镜像源
npm config set registry https://registry.npmjs.org/ # 重置为官方源
npm whoami
```

如果显示以下提示表明还未登录 `npm whoami` 该命令只在登录态下使用，使用 `npm login` 进行登录或者使用`npm adduser`添加新用户

```
npm ERR! code ENEEDAUTH
npm ERR! need auth This command requires you to be logged in.
npm ERR! need auth You need to authorize this machine using `npm adduser`

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\admin\AppData\Roaming\npm-cache\_logs\2021-08-16T02_54_41_525Z-debug.log
```

**使用 `npm login`登录前，必须保证自己在npm官网注册了账号，[注册地址](https://www.npmjs.com/signup)**

```bash
$ npm login
Username: edwardchen1993
Password: xxx
Email: (this IS public) 872990547@qq.com
Logged in as edwardchen1993 on https://registry.npmjs.org/.
```

发布：

```
npm publish
```

发布成功后，可以在 [npm](https://www.npmjs.com/settings/edwardchen1993/packages) 查看到自己刚发布的包。

最后，其他人就可以通过 `npm install do1-cli -g` 进行全局安装进行使用了！

