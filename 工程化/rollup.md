[TOC]

# rollup

rollup 是一个 **Javascript 模块打包器**，可以将小块代码编译成大块复杂的代码，例如 libray 或应用程序，rollup 使用 **ES6** 版本中标准化格式。

+ rollup 用来打包核心库，也可以替代 webpack 打包项目

+ rollup 核心概念 `input` ， `ouput` ，`format` ，`plugins`

+ `plugins` 的使用方式：顺序引用，顺序执行。与 webpack 相反，webpack 是顺序引用，逆序执行。

  

## **概述**

- 仅仅是一款ESM的合并、打包器，并无奇他额外的类似于webpack的功能

- 更为小巧

- Rollup 中并不支持类似 HMR 这种高级特性

- Rollup 并不是要与 Webpack 全面竞争

- 提供一个充分利用ESM各项特性的高效打包器

  

## **快速上手**

### **命令行方式**

- 安装`yarn add rollup --dev`

- 使用 `yarn rollup`，即不传递任何参数的情况下，会出现帮助信息

- `yarn rollup ./src/index.js --format iife --file dist/bundle.js`

- - 第一个参数是入口文件
  - 第二个参数是输出格式，上述采用的是自执行函数的格式
  - 第三个参数是输出的文件路径

- 输出的结果只会保留用到的部分，会自动使用tree-shaking的方式打包代码，十分简洁

### **配置文件方式**

- 新建`rollup.config.js`文件

```js
export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'iife'
  }
}
```

- 运行`yarn rollup --config`来实现以配置文件的形式启动，还可以后面指定不同环境的入口文件

- - `yarn rollup --config rollup.prod.js`
  - `yarn rollup --config rollup.dev.js`



### **使用插件**

- 加载其他类型的文件

- 导入 CommonJS 模块

- 编译 ECMAScript 新特性

- Rollup 支持使用插件的方式扩展

- 插件是Rollup唯一扩展途径，而webpack有loader，plugin，minimizer三种

- 使用：

- - 以导入json文件的插件为例

  - - 安装：`yarn add rollup-plugin-json --dev`
    - 配置文件中进行配置

```js
// rollup.config.js
import json from 'rollup-plugin-json' // 默认导出的是一个函数

export default {
 input: 'src/index.js',
 output: {
 file: 'dist/bundle.js',
 format: 'iife'
  },
 plugins: [
 json()  // 将模块暴露的方法函数的执行结果放进数组中
  ]
}


// index.js
// 导入模块成员，默认情况下rollup只能用路径的方式导入模块，并不能像webpack那样用名称导入node_module中的npm模块
import { log } from './logger'
import messages from './messages'
import { name, version } from '../package.json'

// 使用模块成员
const msg = messages.hi

log(msg)

log(name)
log(version)
```

- - 没有用到的属性会被tree-shaking去掉，自动具有tree-shaking功能



### **Rollup 加载NPM 模块**

- 导入模块成员，默认情况下rollup只能用路径的方式导入模块，并不能像webpack那样用名称导入node_module中的npm模块
- 安装`rollup-plugin-node-resolve`

```js
// rollup.config.js
import json from 'rollup-plugin-json'
import resolve from 'rollup-plugin-node-resolve'

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'iife'
  },
  plugins: [
    json(),
    resolve()
  ]
}

// index.js
import _ from 'lodash-es'  // 可以使用名称的方式导入了
```



### **Rollup 加载 CommonJS 模块**

- rollup默认只支持ESM模块的打包，不支持CommonJS模块
- 但是目前还是有很多模块使用CommonJS语法来导出成员所以需要配置插件来解决这个问题
- 安装`rollup-plugins-commonjs`，在配置文件中配置，然后就能识别引入以`modul.exports`导出的使用CommJS模块了

```js
import json from 'rollup-plugin-json'
import resolve from 'rollup-plugin-node-resolve'
import commonjs from 'rollup-plugin-commonjs'

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'iife'
  },
  plugins: [
    json(),
    resolve(),
    commonjs()
  ]
}

```



### **Rollup 代码拆分**

- 要使用代码拆分就不能使用iife的导出方式，得用amd或者CommonJS

- 需要在配置文件中奖输出方式使用amd，输出目录浏览器环境使用AMD

- 并且也不能使用file方式来配置，因为输出文件不是一个，所以得配置dir设置输出目录

- - 这样就会在dir设置的dist目录里面输出打包后的文件和引入的amd格式的文件了

```js
import('./logger').then(({ log }) => {
 log('code splitting~')
})

export default {
import commonjs from 'rollup-plugin-commonjs'
t/bundle.js',
 // format: 'iife'
 dir: 'dist',
 format: 'amd'
 }
}
```



### **Rollup 多入口打包**

- 将输入路径以数组或者对象的形式进行定义
- 多入口打包会自动提取公共模块，所以不能使用iife了，需要改成amd
- 然后运行`yarn rollup --config`进行打包

```js
export default {
  input: {
    foo: 'src/index.js',
    bar: 'src/album.js'
  },
  output: {
    // file: 'dist/bundle.js',
    // format: 'iife'
    dir: 'dist',
    format: 'amd'
  }
}
```

- 对于AMD输出格式的文件我们不能直接引用到页面上，而是需要借助实现AMD标准的库去加载

- - 手动在`dist`目录中创建`index.html`
  - 使用`require.js`库来加载
  - `data-main`属性来指定引入的文件路径

```html
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>Document</title>
</head>
<body>
 <!-- AMD 标准格式的输出 bundle 不能直接引用 -->
 <!-- <script src="foo.js"></script> -->
 <!-- 需要 Require.js 这样的库 -->
 <script src="https://unpkg.com/requirejs@2.3.6/require.js" data-main="foo.js"></script>
</body>
</html>
```



### **Rollup选用原则**

- Rollup的优势

- - 输出结果更加扁平
  - 自动移除未引用代码（tree-shaking）
  - 打包结果依然完全可读，和手写的代码一致，可读性强

- 缺点

- - 加载非ESM的第三方模块比较复杂，需要使用插件
  - 模块最终都被打包到一个函数中，无法实现HMR
  - 浏览器环境中，代码拆分功能依赖AMD库

- 选用原则

- - 开发应用程序，选择webpack

  - - 因为需要大量引入第三方模块，同时需要HMR来提升开发效率
    - 如果项目过大还需要涉及到分包
    - 上述这些rollup都会欠缺

- - 如果我们正在开发一个框架或者类库就选择使用rollup

  - - 开发类库时很少用到三方模块
    - vue和react框架在开发时也都是用的rollup

- - 并不是绝对的选择标准，而是经验法则
  - Webpack大而全，Rollup小而美