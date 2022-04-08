[TOC]

# vue2

## vue 的 ref 和原生 js 获取 dom 的区别

可以用 `querySelector(id)` 获取当前组件中的 dom 节点，但这么做两个缺点：

1. id 必须是全局唯一的，每个组件里的 id 都不能相同（但 ref 不需要）
2. 如果组件更新时将 id 对应的节点删除或替换了，需要重新用 querySelector 获取节点（但 ref 不需要，因为 Vue 会自动更新 ref 对应的节点）