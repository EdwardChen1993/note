[TOC]

# eslint



## 配置文件优先级

如果在同一个目录中有多个配置文件，ESLint将只使用一个。优先顺序如下:

1. `.eslintrc.js`
2. `.eslintrc.cjs`
3. `.eslintrc.yaml`
4. `.eslintrc.yml`
5. `.eslintrc.json`
6. `package.json`



## Configuring Rules

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



## extends 和 plugins 区别

**extends：**

对于不同项目，如果希望使用相同的 rules，直接复制粘贴显然不是一个好方法，一是 rules 太多，配置文件会显得很乱，二是无法同步更新。

推荐使用的方法是把所需的 rules 抽离成一个 npm 包，需要的时候再通过 extends 引用。而且对于这些抽离出来的包，有着统一的命名规范。



**plugins：**

虽然官方提供了很多规则，但是总有覆盖不到的情况。这时候可以使用 plugin 定义自己的规则。引入 plugin 可以理解为引入了额外的 rules，需要在 rules、extends 中定义后才会生效。

