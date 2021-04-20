[TOC]

# yoeman

Yeoman 是一种高效、开源的 Web 应用脚手架搭建系统，意在精简开发过程。Yeoman 因其专注于提供脚手架功能而声誉鹊起，它支持使用各种不同的工具和接口协同优化项目的生成。

官方文档：[链接](https://yeoman.io/)



## 安装

```bash
npm install -g yo
npm install -g generator-generator
```



## 生成脚手架

```bash
yo generator
```

注意：脚手架名要以 generator- 为前缀，如：generator-edward-gulp。



## 执行过程

文档参考：[链接](https://yeoman.io/authoring/running-context.html)



1. 将想要脚手架生成的代码模板复制到/generators/app/templates下。

2. 修改/generators/app/index.js，如下：

   ```js
   prompting() {
     // Have Yeoman greet the user.
     this.log(
       yosay(
         `Welcome to the exquisite ${chalk.red('generator-edward-gulp')} generator!`
       )
     );
   
     const prompts = [
       {
         type: 'confirm',
         name: 'install',
         message: 'Would you like to enable this option?',
         default: true
       }
     ];
   
     return this.prompt(prompts).then(props => {
       // To access props later use this.props.someAnswer;
       this.props = props;
     });
   }
   
   writing() {
     this.fs.copy(
       this.templatePath('**'),
       this.destinationPath('./')
     );
   },
   install() {
     this.installDependencies({
       bower: false  // 不使用默认的 bower 安装依赖，使用 npm
     });
   }
   ```

3. 执行 npm link ，将该项目下的npm依赖包链接到全局。这个命令的作用就是在全局环境下，生成一个符号链接文件，该文件的名字就是package.json文件中指定的模块名。同时我们对此模块的修改会实时反馈在全局目录下。

   这样运行  yo+脚手架名 时（**此时脚手架名不需要generator- 为前缀**），就可以找到这个脚手架名了，如：yo edward-gulp。

   [参考](https://yeoman.io/authoring/)

   ```bash
   npm link
   ```

4. 新建新的项目目录，并执行 yo+脚手架名 ，即会自动生成项目目录和安装依赖。示例如下：

   ```bash
   mkdir generator-test
   cd generator-test
   # 此处脚手架名 edward-gulp 不需要带 generator 前缀
   yo edward-gulp
```
   
   

## 发布脚手架

[文档参考](https://yeoman.io/generators/#content)、[demo参考](https://blog.csdn.net/GotYou_Ac/article/details/108416901)

1. 登录npm

```bash
npm login
```

2. 发布到npm

```bash
npm publish
```

3. 在本地安装自己的脚手架

```bash
npm install -g generator-edward-gulp
```

4. 使用脚手架生成项目

```bash
yo edward-gulp
```

   



