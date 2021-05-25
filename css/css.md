[TOC]

# css

## 单行文本溢出显示省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
```



## 多行文本溢出显示省略号

仅限webkit浏览器或移动端的页面

```css
display: -webkit-box;
-webkit-line-clamp: 2; // 指定超出多少行溢出显示省略号
-webkit-box-orient: vertical;
overflow: hidden;
text-overflow: ellipsis;
```



## 使用flex布局超出部分显示省略号

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



## css禁用鼠标点击事件

```css
pointer-events: none;
```

[链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events)



## 美化滚动条样式

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

**备注：仅限webkit内核浏览器**



## :nth-child和:nth-of-type 区别

:nth-child和:nth-of-type都是CSS3中的伪类选择器，其作用近似却又不完全一样。

相同点：

```html
<section>
    <p>我是第1个p标签</p>
    <p>我是第2个p标签</p>  <!-- 希望这个变红 -->
</section>
```

```css
p:nth-child(2) { color: red; }
```

```css
p:nth-of-type(2) { color: red; }
```

![CSS3 :nth-child和:nth-of-type选择器差异第一个实例截图](http://image.zhangxinxu.com/image/blog/201106/2011-06-21_214710.png)

上面这个例子中，这两个选择器所实现的效果是一致的，第二个`p`标签的文字变成了红色。



不同点：

```html
<section>
    <div>我是一个普通的div标签</div>
    <p>我是第1个p标签</p>
    <p>我是第2个p标签</p>  <!-- 希望这个变红 -->
</section>
```

```css
p:nth-child(2) { color: red; }
```

![CSS3 :nth-child和:nth-of-type选择器差异实例页面截图](http://image.zhangxinxu.com/image/blog/201106/2011-06-21_220508.png)

`p:nth-child(2)`悲剧了，其渲染的结果不是第二个`p`标签文字变红，而是第一个`p`标签，也就是父标签的第二个子元素。



```css
p:nth-of-type(2) { color: red; }
```

![CSS3 :nth-child和:nth-of-type选择器差异实例页面截图 张鑫旭-鑫空间-鑫生活](http://image.zhangxinxu.com/image/blog/201106/2011-06-21_220802.png)

`p:nth-of-type(2)`的表现显得很坚挺，其把希望渲染的第二个`p`标签染红了。



总结：

对于`p:nth-child(2)`表示**这个元素要是`p`标签，且是第二个子元素**，是两个必须满足的条件。

而`p:nth-of-type(2)`表示**父标签下的第二个`p`元素**。



## :root

:root 这个 CSS 伪类匹配文档树的根元素。对于 HTML 来说，:root 表示<HTML>元素，除了优先级更高之外，与 html 选择器相同。

在声明全局 CSS 变量时 :root 会很有用：

```css
:root {
  --main-color: hotpink;
  --pane-padding: 5px 42px;
}

div {
  background-color: var(--main-color);
  color: var(--pane-padding);
}
```



## line-height继承

| 设置方式 | line-height | 父元素（假设font-size为16px）line-height | 子元素（假设font-size为32px）line-height |
| -------- | ----------- | ---------------------------------------- | ---------------------------------------- |
| inherit  | inherit     | 父元素的父元素line-height值              | 父元素的line-height值                    |
| length   | 20px        | 20px                                     | 20px                                     |
| %        | 120%        | 16px*120%=19.2px                         | 继承父元素计算后的line-height值19.2px    |
| em       | 1.2em       | 16px*1.2em=19.2px                        | 继承父元素计算后的line-height值19.2px    |
| number   | 1.2         | 16px*1.2=19.2px                          | 32px*1.2=38.4px                          |

**总结：**

- **当line-height的属性值有单位时（如px、%、em），子元素继承了父元素计算得出的行高。**
- **当line-height的属性值无单位时，父元素和子元素会分别计算各自的行高（推荐使用）。**



## 父元素选择器

例：为 p 的父元素 li 设置一个边框：

```css
!li > p { border:1px solid #CCC; }
```

或者：

```css
li:has( > p){ border:1px solid #CCC; }
```

**注意：以上皆为提案，均未在浏览器中实现。**
