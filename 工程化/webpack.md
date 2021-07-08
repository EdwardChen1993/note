[TOC]

# webpack



## plugins

plugins 是一个具有 apply 属性的 JavaScript 对象。apply 属性会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问。

**ConsoleLogOnBuildWebpackPlugin.js**

```js
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
    apply(compiler) {
        compiler.hooks.run.tap(pluginName, compilation => {
            console.log("webpack 构建过程开始！");
        });
    }
}
```

compiler hook 的 tap 方法的第一个参数，应该是驼峰式命名的插件名称。建议为此使用一个常量，以便它可以在所有 hook 中复用。



## loader和plugins区别

**loader：**

让webpack能够处理非js文件(如css、图片、字体)，将其转换成webpack能处理的有效文件，单纯的文件转换过程。例如：`css-loader`、`style-loader`、`postcss-loader`、`sass-loader`。

**plugins：**

赋予webpack可以无限扩展的能力，可以做更加复杂类型的任务。包括打包压缩、生成html文件、重新定义环境变量等。例如：`uglify-webpack-plugin`、`clean-webpack-plugin`、`babel-polyfill`、`DefinePlugin`。

plugins针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务



## mode

webpack4.0以后，提供 `mode` 配置选项，告知 webpack 使用相应模式的内置优化。

### 用法

只在配置中提供 `mode` 选项：

```js
module.exports = {
  mode: 'production'
};
```

或者从 [CLI](https://www.webpackjs.com/api/cli/) 参数中传递：

```bash
webpack --mode=production
```



支持以下字符串值：

| 选项          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| `development` | 会将 `process.env.NODE_ENV` 的值设为 `development`。启用 `NamedChunksPlugin` 和 `NamedModulesPlugin`。 |
| `production`  | 默认值，会将 `process.env.NODE_ENV` 的值设为 `production`。启用 `FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `OccurrenceOrderPlugin`, `SideEffectsFlagPlugin` 和 `UglifyJsPlugin`. |
| `none`        | 不使用任何默认优化选项。                                     |



## 调试webpack配置文件

### 方法一、直接使用console.log。



### 方法二、使用node。

**命令行输入：**

```bash
node --inspect-brk ./node_modules/.bin/webpack --inline --progress
```



**在chrome浏览器地址输入：**

```bash
chrome://inspect/#devices
```



点击target列表中的inspect，即可进入调试界面。



### 方法三、使用vscode debugger。

​		

## 压缩js的plugin

webpack v4 开始从 uglify 转为 terser





