[TOC]

# vue3

## 与vue2相比

**性能提升**

+ 与vue2相比，mount 50%提升，内存占用小120%

**框架体积**

+ 核心代码 + Composition API：13.5kb，最小11.75kb

+ 所有Runtime：22.5kb（vue2是32kb）

**新特性**

+ TS重写Diff算法，使用Proxy（所有IE浏览器均不支持）性能更优，框架体积更小
+ 新的Compiler，通过注释标记提升框架性能
+ Composition API，模块化功能代码，摒弃this
+ 更好的按需加载（得益于Tree Shaking）
+ 新增：Fragment、Teleport、Suspense
+ Vite开发工具

