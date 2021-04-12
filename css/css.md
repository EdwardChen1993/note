[TOC]

# 单行文本溢出显示省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
```



# 多行文本溢出显示省略号

仅限webkit浏览器或移动端的页面

```css
display: -webkit-box;
-webkit-line-clamp: 2; // 指定超出多少行溢出显示省略号
-webkit-box-orient: vertical;
overflow: hidden;
text-overflow: ellipsis;
```



# 使用flex布局超出部分显示省略号

```css
.father {
  display: flex;
  flex: 1;
  overflow: hidden; // 必须，否则不生效
}
.son {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

[链接](https://blog.csdn.net/Sophiaxnm/article/details/105860914)



# css禁用鼠标点击事件

```css
pointer-events: none;
```

[链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events)



# 美化滚动条样式

仅限webkit浏览器

```css
// 整个滚动条
&::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
// 滚动条没有滑块的轨道部分
&::-webkit-scrollbar-track-piece {
  background-color: $background-color--main;
}
// 滚动条上的滚动滑块
&::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background-color: #ced1d7;
  &:hover {
    background-color: #999;
  }
}
```

[链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-scrollbar)

