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
| %        | 120%        | 16px * 120% = 19.2px                     | 继承父元素计算后的line-height值19.2px    |
| em       | 1.2em       | 16px * 1.2em = 19.2px                    | 继承父元素计算后的line-height值19.2px    |
| number   | 1.2         | 16px * 1.2 = 19.2px                      | 32px * 1.2 = 38.4px                      |

**总结：**

- **当line-height的属性值有单位时（如px、%、em），子元素继承了父元素计算得出的行高。**
- **当line-height的属性值无单位时，父元素和子元素会分别计算各自的行高（推荐使用）。**



## 父元素选择器

例：为子元素 p 的父元素 li 设置一个边框：

```css
!li > p { border:1px solid #CCC; }
```

或者：

```css
li:has(> p) { border:1px solid #CCC; }
```

**注意：以上皆为提案，均未在浏览器中实现。**



## currentColor

currentColor 关键字代表原始的 color 属性的计算值。它允许让继承自属性或子元素的属性颜色属性以默认值不再继承。

currentColor是CSS3中的变量，它表示“当前的标签所继承的文字颜色”。“当前颜色” 指本体color ，如果没有设置color就找父元素，一级一级找,一直到根元素位置。

示例：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .father {
        width: 200px;
        height: 200px;
        color: yellow;
        border: 1px solid red;
      }

      .son {
        width: 100px;
        height: 100px;
        color: blue;
        border: 1px solid currentColor;
      }
    </style>
  </head>

  <body>
    <div class="father">
      <div class="son"></div>
    </div>
  </body>
</html>
```

当子元素设置了color属性为blue时，子元素的border颜色为blue。如果子元素没有设置color属性，子元素的border颜色为父元素的color属性的yellow。



## css媒体属性屏幕方向（orientation）

css 媒体属性屏幕方向（orientation）可用于测试视口 viewport（或者对于分页媒体而言的页面框盒）的横纵方向。

orientation 属性被指定为下列关键字值中的任意一个：

**portrait：**viewport 处于纵向，即高度大于等于宽度。

**landscape：**viewport 处于横向，即宽度大于高度。

示例：

```css
body {
  display: flex;
}

div {
  background: yellow;
}

@media (orientation: landscape) {
  body {
    flex-direction: row;
  }
}

@media (orientation: portrait) {
  body {
    flex-direction: column;
  }
}
```



## content属性使用中文出现乱码

在进行页面开发时，经常会使用:before, :after伪元素创建一些小tips，但是在:before或:after的content属性使用中文的话，会导致某些浏览器上出现乱码。

避免在CSS的:before, :after中使用中文，如果一定要使用，可以使用中文对应的Unicode。可以使用使用工具进行Unicode编码转换，或者是JavaScript的原生方法escape将中文转为Unicode。

需要注意的是Unicode在CSS中的书写方式，例如“你好啊”对应的Unicode是 `\u4F60\u597D\u554A`，而在CSS中要写成 span:before { content: '\4F60\597D\554A'' } 。



## flex布局

**justify-content：**

+ space-between：在每行上均匀分配弹性元素。相邻元素间距离相同。每行第一个元素与行首对齐，每行最后一个元素与行尾对齐。

+ space-around：每行上均匀分配弹性元素。相邻元素间距离相同。每行第一个元素到行首的距离和每行最后一个元素到行尾的距离将会是相邻元素之间距离的一半。
+ space-evenly：flex项都沿着主轴均匀分布在指定的对齐容器中。相邻flex项之间的间距，主轴起始位置到第一个flex项的间距,，主轴结束位置到最后一个flex项的间距，都完全一样。



## var()

`var()` 函数可以代替元素中任何属性中的值的任何部分。`var()`函数不能作为属性名、选择器或者其他除了属性值之外的值。（这样做通常会产生无效的语法或者一个没有关联到变量的值。）

方法的第一个参数是要替换的自定义属性的名称。函数的可选第二个参数用作回退值。如果第一个参数引用的自定义属性无效，则该函数将使用第二个值。

```css
var( <custom-property-name> , <declaration-value>? )
```

**custom-property-name 自定义属性名**

在实际应用中它被定义为以两个破折号开始的任何有效标识符。 自定义属性仅供作者和用户使用; CSS 将永远不会给他们超出这里表达的意义。

**declaration-value 声明值（后备值）**

回退值被用来在自定义属性值无效的情况下保证函数有值。回退值可以包含任何字符，但是部分有特殊含义的字符除外，例如换行符、不匹配的右括号（如)、] 或 }）、感叹号以及顶层分号（不被任何非var**()**的括号包裹的分号，例如 `var(--bg-color, --bs**;**color)` 是不合法的，而 `var(--bg-color, --value**(**bs;color**)**)` 是合法的）。

**示例：**

```css
:root {
  --main-bg-color: pink;
}

body {
  background-color: var(--main-bg-color);
}

/* 后备值 */
/* 在父元素样式中定义一个值 */
.component {
  --text-color: #080; /* header-color 并没有被设定 */
}

/* 在 component 的样式中使用它： */
.component .text {
  color: var(--text-color, black); /* 此处 color 正常取值 --text-color */
}
.component .header {
  color: var(--header-color, blue); /* 此处 color 被回退到 blue */
}
```



## env()

`env()` CSS 函数以类似于 `var()` 函数和 custom properties 的方式将用户代理定义的环境变量值插入你的 CSS 中。区别在于，环境变量除了由用户代理定义而不是由用户定义外，还被全局作用在文档中，而自定义属性则限定在声明它们的元素中。

为了告诉浏览器使用屏幕上所有的可用空间，并以此使用 `env()` 变量，我们需要添加一个新的视口元值：

```html
<meta name="viewport" content="... viewport-fit=cover">
```

```css
body {
  padding:
    env(safe-area-inset-top, 20px)
    env(safe-area-inset-right, 20px)
    env(safe-area-inset-bottom, 20px)
    env(safe-area-inset-left, 20px);
}
```

另外，与自定义属性不同，自定义属性不能在声明之外使用，而`env()`函数可以代替属性值或描述符的任何部分（例如，在 媒体查询的规则 中）。 随着规范的发展，它也可能在像是 选择器 等其他地方使用。

最初由iOS浏览器提供，用于允许开发人员将其内容放置在视口的安全区域中，该规范中定义的`safe-area-inset-*`值可用于确保内容即使在非矩形的视区中也可以完全显示。

**示例：**

```css
/* Using the four safe area inset values with no fallback values */
env(safe-area-inset-top);
env(safe-area-inset-right);
env(safe-area-inset-bottom);
env(safe-area-inset-left);

/* Using them with fallback values */
env(safe-area-inset-top, 20px);
env(safe-area-inset-right, 1em);
env(safe-area-inset-bottom, 0.5vh);
env(safe-area-inset-left, 1.4rem);
```

**值：**

- `safe-area-inset-top`, `safe-area-inset-right`, `safe-area-inset-bottom`, `safe-area-inset-left`。
- `safe-area-inset-*` 由四个定义了视口边缘内矩形的 top, right, bottom 和 left 的环境变量组成，这样可以安全地放入内容，而不会有被非矩形的显示切断的风险。对于矩形视口，例如普通的笔记本电脑显示器，其值等于零。 对于非矩形显示器（如圆形表盘，iPhoneX屏幕），在用户代理设置的四个值形成的矩形内，所有内容均可见。

注意: 不同于其他的 CSS 属性，用户代理定义的属性名字对大小写敏感。
