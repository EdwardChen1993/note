[TOC]

# lint-staged

lint-staged 是一个在git暂存文件上运行linters的工具。在代码提交之前，进行代码规则检查能够确保进入git库的代码都是符合代码规则的。但是整个项目上运行lint速度会很慢，lint-staged 能够让lint只检测暂存区的文件，所以速度很快。

[参考文档](https://github.com/okonet/lint-staged#readme)

## 安装

```bash
npm install lint-staged -D 
```



## 运行

**建议配合`husky`一起使用，详情见`husky.md`—配置一节。**

```bash
 npx lint-staged
```



## 配置

可以在`package.json`文件的 lint-staged 对象中写入相应的命令即可：

```json
"lint-staged": {
  "src/**/*.{js,vue}": [
    "vue-cli-service lint",
    "git add"
  ],
  "src/**/*.{js,vue,css,scss,less,html}": [
    "prettier --no-semi --single-quote --write",
    "git add"
  ]
}
```

lint-staged 使用 **glob模式** 来匹配对应规则的文件，lint-staged 指定的命令会处理匹配的所有暂存文件。

键名为glob模式匹配的规则，键值为执行的命令。

