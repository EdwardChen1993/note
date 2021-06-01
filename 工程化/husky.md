[TOC]

# husky

husky是一个npm包，可以将git内置的钩子暴露出来，很方便地进行钩子的命令注入，而不需要在.git目录下自己写shell脚本了；不仅可以执行js文件作为脚本，还可以将脚本暴露出来，方便在git项目中进行管理。

[参考文档](https://typicode.github.io/husky/#/)



## 原理

husky支持的钩子就是[git hooks](https://git-scm.com/docs/githooks)，可以在对应钩子指定一条shell命令，husky会自动将这个命令写入.git目录下的对应钩子脚本，当触发这个git钩子时则会执行我们写入的命令；

因此husky本质上就是执行shell命令，因此只要是shell命令都可以在钩子中执行；由于nodeJS也可以通过命令行执行，所以直接用js来充当脚本也是可以的；



## 安装

```bash
npm install husky -D
```



## 配置

可以在**package.json**文件的husky.hooks对象中写入相应的命令即可，键名就是钩子名称，键值就是需要执行的命令，如：

```json
"husky": {
  "hooks": {
    "pre-commit": "lint-staged",
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
}
```

**常用钩子：**

+ pre-commit：由git commit命令触发，在commit-msg之前；

- commit-msg：git commit和git merge都会触发，会传递一个参数，该参数为存放当前commit消息的临时文件路径；可以通过--no-verify参数来跳过commit-msg钩子；
- post-merge：触发于merge完成后；
