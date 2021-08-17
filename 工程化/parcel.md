[TOC]

# Parcel

零配置的前端应用打包器

- 使用

- - 首先初始化一个包管理系统`yarn init -y`

  - 安装 `yarn add parcel-bundler --dev`

  - 和webpack不同的是parcel建议使用**html文件**作为打包入口

  - 运行

  - - `yarn parcel src/index.html` 参数是入口文件路径

- - 功能

  - - 不仅会打包文件还会启动一个相当于webpack中devServer一样的工具，并具有自动刷新功能

    - 如果想要使用HMR（自动热替换）而不是自动刷新，那么他也支持使用module的accept方法来实现

    - - 和webpack不同的是，webpack支持两个参数，第二个参数是函数用来执行热替换后的行为
      - 而parcel中的module中的accept方法只能接受一个参数，也就是为哪个文件执行热替换

- - - 自动安装依赖

    - - 我们只管在入口文件中导入第三方包，parcel会自动帮我们导入用到的依赖

- - - 加载其他类型的资源模块

    - - 可以直接导入css样式文件或者图片文件，不需要安装额外的loader和插件

- - - 支持动态导入

    - - 只管用即可，内部会自动拆分代码，这个动态导入的代码只有在用到的时候才会请求

  - ```js
    // 动态导入jQuery
    import ('jquery').then( $ => {
     $(document.body).append('<h1>hello parcel</h1>')
    })
    ```

- - - 以生产模式进行打包

    - - `yarn parcel build src/index.html`

      - 相同体量的代码parcel会比webpack效率高很多

      - - 因为parcel是以多线程进行工作的，充分发挥了多核CPU的性能
        - webpack中也可以使用`happypack`的插件来实现多线程打包

- - - 打包的结果都会被压缩，并且样式代码也都会被提取到单独的文件中

- - 优点

  - - 由于webpack配置过于繁琐，所以parcel应运而生
    - 实现了完全零配置打包
    - 构建速度快

- - 与webpack对比

  - - webpack生态更好
    - webpack越来越好用
    - 对parcel的学习更多的是保持对新技术的敏感度，了解技术走向，目前广泛应用的还是webpack
