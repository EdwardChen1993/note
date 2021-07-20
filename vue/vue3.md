[TOC]

# vue3

## 与 vue2 相比

### 性能提升

+ 与vue2相比，mount 50%提升，内存占用小120%

### **框架体积**

+ 核心代码 + Composition API：13.5kb，最小11.75kb

+ 所有Runtime：22.5kb（vue2是32kb）

### **新特性**

+ TS重写Diff算法，使用Proxy（所有IE浏览器均不支持）性能更优，框架体积更小
+ 新的Compiler，通过注释标记提升框架性能
+ Composition API，模块化功能代码，摒弃this
+ 更好的按需加载（得益于Tree Shaking）
+ 新增：Fragment、Teleport、Suspense
+ Vite开发工具

### **从 vue2 迁移**

+ 2.x 版本中在一个元素上同时使用 `v-if` 和 `v-for` 时，`v-for` 会优先作用。3.x 版本中 `v-if` 总是优先于 `v-for` 生效。
+ 生命周期命名变化，`destroy` -> `unmounted`，`beforeDestroy` -> `beforeUnmount`





## Composition API

### 原因

vue2逻辑复用方式使用 `Mixin`，会造成命名空间冲突、逻辑不清晰、不易复用。同时 Options API 使用 (`data`、`computed`、`methods`、`watch`) 组件选项来组织逻辑通常都很有效。然而，当我们的组件开始变得更大时，**逻辑关注点**的列表也会增长。尤其对于那些一开始没有编写这些组件的人来说，这会导致组件难以阅读和理解。

这种碎片化使得理解和维护复杂组件变得困难。选项的分离掩盖了潜在的逻辑问题。此外，在处理单个逻辑关注点时，我们必须不断地“跳转”相关代码的选项块。

如果能够将同一个逻辑关注点相关代码收集在一起会更好。而这正是 Composition API 使我们能够做到的。



### 优点

+ 逻辑代码更少，更集中，更易扩展
+ 更加丰富的API集成
+ 对于TS来说，非常友好（利于类型推导）



### 对比

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage1.bubuko.com%2Finfo%2F202102%2F20210203233241974982.png&refer=http%3A%2F%2Fimage1.bubuko.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1628476905&t=ae7d55e44c879a6a01a8f6e1fa419b0b)



### Setup

使用 `setup` 函数时，它将接收两个参数：

1. `props`
2. `context`

#### Props

`setup` 函数中的第一个参数是 `props`。正如在一个标准组件中所期望的那样，`setup` 函数中的 `props` 是响应式的，当传入新的 prop 时，它将被更新。

```js
// MyBook.vue

export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

**注意：因为 `props` 是响应式（即 `proxy` 对象）的，你不能使用 ES6 解构，它会消除 prop 的响应性。**



#### Context

传递给 `setup` 函数的第二个参数是 `context`。`context` 是一个普通的 JavaScript 对象，它暴露组件的三个 property：

```js
// MyBook.vue

export default {
  setup(props, context) {
    // Attribute (非响应式对象)
    console.log(context.attrs)

    // 插槽 (非响应式对象)
    console.log(context.slots)

    // 触发事件 (方法)
    console.log(context.emit)
  }
}
```

`context` 是一个普通的 JavaScript 对象，也就是说，它不是响应式（即非 `proxy` 对象）的，这意味着你可以安全地对 `context` 使用 ES6 解构。

```js
// MyBook.vue
export default {
  setup(props, { attrs, slots, emit }) {
    ...
  }
}
```



#### 渲染函数

`setup` 还可以返回一个渲染函数，该函数可以直接使用在同一作用域中声明的响应式状态：

```js
// MyBook.vue

import { h, ref, reactive } from 'vue'

export default {
  setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })
    // 请注意这里我们需要显式调用 ref 的 value
    return () => h('div', [readersNumber.value, book.title])
  }
}
```

**备注：渲染函数可以配合 `jsx` 使用，如下：**

```js
import AlertComponent from '../components/AlertComponent'
export default {
	setup() {
  	return () => (<AlertComponent />)
	}
}
```



#### this

因为 `setup` 执行时（**执行时机比 `beforeCreate` 和 `created` 要早**），尚未创建组件实例，所以 `setup` 选项中没有 `this` ，无法访问组件声明的属性。



#### watch vs watchEffect

区别：

+ 都可以监听响应式对象的变化，从而执行回调
+ `watchEffect` 会默认执行一次，而  `watch` 不会，必须事件触发
+ `watchEffect` 只接受函数作为回调参数



## Suspence

`Suspence`可以用于等待某个异步组件解析完成的前后的显示情况。比如一个组件是图片，它里面是图片资源是网络请求过来的，我们就能使用 `Suspense` 展示在这个请求未完成的情况下展示的另外一种样式。

**父组件**

```vue	
<template>
  <div v-if="errMsg">
    {{errMsg}}
  </div>
  <Suspense v-else>
    <template #default>
      <!-- 异步组件，当 setup 方法中的异步方法执行完后显示 -->
        <HelloWorld />
    </template>
    <template #fallback>
      <!-- 默认加载的内容，比如 loading 提示或者图标 -->
      loading
    </template>
  </Suspense>
  <HelloWorld />
</template>
<script>
import HelloWorld from '@/components/HelloWorld.vue'
import { ref, onErrorCaptured } from '@vue/runtime-core'
export default {
  components: {
    HelloWorld
  },
  setup () {
    const errMsg = ref('')
    // 捕获子组件 promise reject 的错误
    onErrorCaptured((error) => {
      errMsg.value = error.message
      console.log('App error', error)
    })
    return {
      errMsg
    }
  }
}
</script>
```

**子组件**

```vue
<template>
  <div>
    Hello world
  </div>
</template>
<script>
export default {
  async setup () {
    const p = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('resolve')
        // reject(new Error('reject'))
      }, 2000)
    })
    const res = await p
    return {
      res
    }
  }
}
</script>
```

当 `setup` 内的 `promise ` 尚未 `resolve` 或者 `reject` 时，显示 `#fallback` 的内容。当 `resolve` 时，显示 `#default` 的内容。当 `reject` 时， `#default` 和 `#fallback` 均不显示，但会触发 `onErrorCaptured` 钩子。

**注意：子组件的`setup` 需要指定为 `async`。**



## Teleport

`Teleport` 用于控制在 DOM 中任一指定父节点下呈现 HTML，而不必求助于全局状态或将其拆分为两个组件。

+ 控制部分 DOM 脱离根节点
+ 可以使用本地化逻辑控制组件
+ 适用于fixed或绝对定位的组件（如Modal模态框）

`Teleport`的 `to` 属性可以指定的对象格式：

+ id -> `<Teleport to="#id">`
+ class -> `<Teleport to=".className">`
+ data -> `<Teleport to="[data-meta]">`
+ 动态 ->  `<Teleport :to="props">`

`Teleport`的 `disabled ` 属性可用于禁用 `<Teleport>` 的功能，这意味着其插槽内容将不会移动到任何位置，而是在你在周围父组件中指定了 `<Teleport>` 的位置渲染。

**备注：这将移动实际的 DOM 节点，而不是被销毁和重新创建，并且它还将保持任何组件实例的活动状态。所有有状态的 HTML 元素 (即播放的视频) 都将保持其状态。**

`Telepor` 中可以传送多组 DOM ，顺序将是一个简单的追加——稍后挂载将位于目标元素中较早的挂载之后，按照先后顺序 append。



