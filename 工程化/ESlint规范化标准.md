[TOC]

# ESlint规范化标准

规范化是践行前端工程化中重要的一部分



## **为什么要有规范化准备**

- 软件开发需要多人协同
- 不同开发者具有不同的编码习惯和喜好
- 不同的喜好增加项目维护成本
- 每个项目或者团队需要明确统一的标准



## **哪里需要规范化标准**

- 代码、文档、甚至是提交日志

- 开发过程中人为编写的成果物

- 代码标准化规范最为重要

- - 如统一关键词及左右的空格
  - 统一代码的缩进方式
  - 统一是否使用分号结尾
  - 统一变量和函数的命名规范
    ······



## **实施规范化的方法**

- 编码前人为的标准约定
- 通过工具实现Lint



## **ESLint 介绍**

- 最为主流的JavaScript Lint 工具监测JS代码质量
- ESLint 很容易统一开发者的编码风格
- ESLint 可以帮助开发者提升编码能力



## **ESLint 工具使用**

- 初始化项目

- - `npm init -y`

- 安装ESlint：`npm i eslint --save --dev`

- 执行`npx eslint --version` 可以获取执行到eslint模块下的version命令

- - 如果不使用npx则需要到eslint的.bin目录下执行version命令
  - npm和npx区别就是npx可以直接执行模块中的命令而不用切换到.bin目录（yarn也可以）



## **ESlint 快速上手**

- ESlint检查步骤

- - 编写“问题”代码
  - 使用eslint执行检测
  - 完成eslint使用配置



- 生成ESlint配置文件

- - `npx eslint --init`
  - 然后弹出一些交互问题，其中代码风格要选用通用型Use a popular style guide
  - 然后在选用标准型standard风格即可（没有固定标准，根据项目团队来决定）
  - 配置好了所有选项后根目录下会生成一个`.eslintrc.js`的eslint的配置文件
  - 最终可以通过`npx eslint xxx.js`来检测某一文件（或者把npx换成yarn 也可以）
  - 可以通过`npx eslint xxx.js --fix`来修正代码中的部分格式问题**（多数是代码风格上的问题，如缩进空格、空行个数）**



## **ESlint 的配置文件**

- 通过`npx eslint --init` 生成的`.eslintrc.js`文件就是配置文件
- 我们可以通过对这个文件进行配置来实现开启或关闭某项校验规则
- - env // 根据环境判断全局成员是否可用

  - - `browser:true `// 表示代码运行在浏览器环境当中，可以使用 `window` 和 `document`
    - 也就是说每一个环境代表着一组预定义的全局变量的可使用性
- standard风格中的预定义配置
- - 由于我们使用`eslint --init`初始化的文件继承自standard风格中的

  - - 即时我们在初始化文件中将 `browser` 设置成 `false` ，那么我们也可以继续使用 `window` 和 `document`
    - 原因就是standard风格配置中进行了允许多环境配置

  - 在node_module中找到`eslint-config-standard`中的`eslintrc.json`

  - - 通过观察这里面的配置可以发现，`document`、`navigator`、`window`都被设置成全局只读了，所以不能通过`.eslintrc.js`来修改
    - 所有的环境并不是互斥的，也就是说可以同时开启browser、node，这样二者中的变量都可以直接使用了

  - extends // 用来继承共享的配置，可以在多个文件之间共享同一个eslint配置
- 插件名称可以省略 `eslint-plugin-` 前缀
- - standard
  - 可以是个数组定义多个共享配置
  - parserOptions
  - - 语法解析器，表示允许使用ESMA的版本

    - 只是检测语法，不代表某成员是否可用，具体是通过环境env来进行限制，env中的`es6:false`才是真的限制

    - - 比如把 `ParserOptions` 中的 `ecmaVersion` 中设置成 `2015` ，env中的es6设置为`false`
      - 那么这样es6的语法仍然不可以使用，如 `Promise`
  - rules // 表示每个校验规则的开启或者关闭
  - 如设置`'no-alert': 'error' `// 表示当代码中使用了alert就会报出一个错误
  - 有三个属性值分别是`off`，`warning`，`error`，或者使用数字`0`，`1`，`2`代替

- 可用的属性值详见官网列表
- - `globals` // 表示可使用的全局成员

```js
module.exports = {
  env: {
    browser: false,
    es6: false
  },
  extends: [
    'standard'
  ],
  parserOptions: {
    ecmaVersion: 2015
  },
  rules: {
    'no-alert': "error"
  },
  globals: {
    "jQuery": "readonly"
  }
}
```

**说明：globals后面的参数用来表示这个变量的可操作度：**

- readonly/false——只读
- writable/true——可写
- off——**禁用该全局变量**
- true/false 等价于只读/可写，但不推荐使用。



ESLint 附带有大量的规则。你可以使用注释或配置文件修改你项目中要使用的规则。要改变一个规则设置，你必须将规则 ID 设置为下列值之一：

- `"off"` 或 `0` - 关闭规则
- `"warn"` 或 `1` - 开启规则，使用警告级别的错误：`warn` (不会导致程序退出)
- `"error"` 或 `2` - 开启规则，使用错误级别的错误：`error` (当被触发的时候，程序会退出)

示例：

```js
{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}
```



## ESLint 配置文件格式

ESLint 支持几种格式的配置文件：

- **JavaScript** - 使用 `.eslintrc.js` 然后输出一个配置对象。
- **YAML** - 使用 `.eslintrc.yaml` 或 `.eslintrc.yml` 去定义配置的结构。
- **JSON** - 使用 `.eslintrc.json` 去定义配置的结构，ESLint 的 JSON 文件允许 JavaScript 风格的注释。
- **(弃用)** - 使用 `.eslintrc`，可以使 JSON 也可以是 YAML。
- **package.json** - 在 `package.json` 里创建一个 `eslintConfig`属性，在那里定义你的配置。

如果同一个目录下有多个配置文件，ESLint 只会使用一个。优先级顺序如下：

1. `.eslintrc.js`**（推荐：js 文件可以做条件判断）**
2. `.eslintrc.yaml`
3. `.eslintrc.yml`
4. `.eslintrc.json`
5. `.eslintrc`
6. `package.json`



## **ESLint 配置注释**

- 直接通过注释的方式写在我们的脚本文件中然后执行代码的校验

- 可能会在代码中有个别需要违反配置的地方，但是不能为了个别点去推翻整个eslint配置

- 所以可以通过eslint配置注释的方式来解决，去忽视某行中的代码

- - `eslint-disable-line` // 忽略当前行种所有的代码错误的检查
  - `eslint-disable-line no-template-curly-in-string` // 表示忽略当前行种当前规则的检查【推荐】



## **ESlint结合自动化工具的使用**

- 建议ESlint结合自动化工具当中

- 集成之后，ESLint一定会工作

- 与项目统一，管理更加方便，不用分别执行构建和代码检测

- 结合gulp使用ESlint

- - 在github上`git clone`项目
  - 完成项目依赖安装
  - 完成 `eslint` 模块安装
  - 完成 `gulp-eslint` 模块安装
  - 初始化eslint配置文件 `npx eslint --init`
  - 配置gulp

```js
const script = ()=> {
 return ('src/assets/scripts/*.js',{base : 'src'})
    .pipe(plugins.eslint())
 .pipe(plugins.eslint.format())  // 在控制台打印出具体的错误信息
 .pipe(plugins.eslint.failAfterError())  // 检查到错误终止任务管道
    .pipe(plugins.babel({presets:['@babel/preset-env']}))
    .pipe(dest('temp'))
    .pipe(bs.reload({stream: true}))
}
```



## **ESlint 结合 Webpack**

- 是通过loader的方式实现而不是通过插件

第一种方法：

```js
rules: [
  {
    test: /.js$/,
    exclude: /node_modules/,
    use: ['babel-loader', 'eslint-loader'],
  }
]
```

第二种方法（更推荐）：

```js
rules: [
  {
    test: /.js$/,
    exclude: /node_modules/,
    use: 'eslint-loader',
    enforce: 'pre'  // 表示要在其他loader之前执行
  }
]
```

- 流程

- - `git clone`项目
  - 安装对应模块
  - 安装`eslint`模块
  - 安装`eslint-loader`模块
  - 初始化`eslint`配置文件

- 关于React项目的注意事项

- - 由于react项目中引入了React包用来支持JSX语法，但是项目中并没有使用到

  - 所以会报错，可以使用一个插件来解决`yarn add eslint-plugin-react --dev`

  - 然后在`.eslintrc.js`中添加`plugins`属性，属性值是数组，去掉模块名的`eslint-plugin`即可

  - - `plugins: ['react']`

- - 还需要在rules中进行配置

  - - `0`表示`off`，`1`表示`warning`，`2`表示`error`
    - `rules: {'react/jsx-uses-react' : 2}` // 避免React引入不使用而产生的报错
    - `rules : {'react/jsx-uses-react' : 2，'react/jsx-uses-vars': 2}` // 这个避免组件引入不使用而导致的报错

- - 或者我们直接在extends里面添加一个继承配置即可，并去除上述plugins和rules中的配置

  - - `extends：['standard', 'plugin:react/recommended']`



## **现代化项目集成ESlint**

以Vue为例

- `npm install @vue/cli -g`
- `vue create vue-app`
- 在交互命令行选择时选择`lint`和`standard`的校验风格
- lint操作环节选择`lint on save`和`lint and fix on commit`
- In dedicated config files // 独立文件单独存放



## **ESlint对TypeScript的支持**

- 安装typescript

- 初始化eslint，交互命令行中的TS选项要开启，并会询问是否安装两个校验TS包

- 配置`.eslintrc.js`

- - 需要额外配置一个用于解析ts语法的语法解析器

```js
module.exports = {
  env: {
    browser: true,
    es2020: true
  },
  extends: [
    'standard'
  ],
  parser: '@typescript-eslint/parser',  // 配置一个用于解析ts语法的语法解析器
  parserOptions: {
    ecmaVersion: 11
  },
  plugins: [
    '@typescript-eslint'
  ],
  rules: {
  }
}
```



## **Stylelint工具的使用**

- Stylelint 使用介绍

- - 提供默认的代码检查规则
  - 提供CLI工具，快速调用
  - 通过插件支持 Sass Less PostCSS
  - 支持Gulp和webpack的集成使用

- 使用

- - 安装

  - - `npm install stylelint -D`
    - `npm install stylelint-config-standard -D` // 安装一个共享配置模块

  - 配置

  - - 新建一个`.stylelintrc.js`的配置文件，共性配置名称和ESlint不同，需要时完整的名称

```js
module.exports = {
 extends: "stylelint-config-standard"
}
```

- - 执行

  - - `npx stylelint ./index.css`

- 关于sass文件的代码检测

- - 安装 `npm install stylelint-config-sass-guidelines -D`

- 配置

```js
module.exports = {
 extends: [
 	"stylelint-config-standard",
 	"stylelint-config-sass-guidelines" // 安装sass的共享配置模块
 ]
}
```

- - 执行

  - - `npx stylelint ./index.sass`



## **Prettier**

- 代码格式化工具，前端规范化标准

- 使用

- - 安装

  - - `npm install prettier -D`

  - 执行

  - - `npx prettier style.css` // 默认会把格式化后的代码输出到命令行当中
    - `npx prettier style.css --write` // 这样格式化后的代码就会覆盖掉源文件，而不是出现在命令行中

  - 将所有文件都执行格式化

  - - `npx prettier . --write` // 将根目录下的所有文件都进行格式化



## **Git Hooks**

- - 由于编码人员的疏忽，有可能代码提交至仓库之前未执行 lint 工具
  - 所以可以通过 Git Hooks 在代码提交前强制 lint

- 简介

- - Git Hook 也称之为git钩子，每个钩子都对应一个任务
  - 通过 shell 脚本可以编写钩子任务触发时要具体执行的操作

- ESLint 结合 Git Hooks

- - 很多前端开发者并不擅长使用shell

  - Husky 可以实现 Git Hooks 的使用需求

  - husky使用

  - - `npm install husky -D`
    - 在`package.json`中进行配置

```js
"husky": {
 "hooks": {
 "pre-commot": "npm run test" // 在git commit之前会触发执行test
  }
}
"scripts": {
 "test": "eslint ./index.js" // test会以eslint的方式进行校验
}
```

- - - 经过上述配置就能实现代码在commit之前进行强制eslint校验了，但是并不能将其格式化及其他后续操作
    - 引出`lint-staged`模块

- lint-staged

- - 安装 `npm install lint-staged -D`
  - 在`package.json`中增进行配置

```js
"lint-staged": {
 "*.js": [
 "eslint",  // 需要完成的后续任务
 "git add"
  ]
}
"scripts": {
 "test": "eslint ./index.js",
 "precommint": "lint-staged"
}
"husky": {
 "hooks": {
 "pre-commot": "npm run precommit"
  }
}
```

- - 执行

  - - `git commit -m "test"`
    - 这样就是husky配合着lint-staged使用，当进行commit之前会执行`npm run precommit`
    - 然后precommit中定义了会执行`eslint`和`git add`



## extends 和 plugins 区别

**extends：**

对于不同项目，如果希望使用相同的 `rules`，直接复制粘贴显然不是一个好方法，一是 `rules` 太多，配置文件会显得很乱，二是无法同步更新。

推荐使用的方法是把所需的 `rules `抽离成一个 npm 包，需要的时候再通过 `extends ` 引用。而且对于这些抽离出来的包，有着统一的命名规范。

**plugins：**

虽然官方提供了很多规则，但是总有覆盖不到的情况。这时候可以使用 `plugin `定义自己的规则。引入 `plugin` 可以理解为引入了额外的 `rules`，需要在 `rules`、`extends `中定义后才会生效。
