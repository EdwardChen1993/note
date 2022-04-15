[TOC]

# React Testing Library

- [GitHub 仓库](https://github.com/testing-library/react-testing-library)
- [官方文档](https://testing-library.com/docs/react-testing-library/intro/)
- 基于 [DOM Testing Library](https://testing-library.com/docs/dom-testing-library/intro) 构建，用于测试 React 组件
- 核心思想：测试越类似于您的软件使用方式，它们就可以给您带来更多的信心
- 工作原理：将 React 组件渲染为 DOM 文档，测试的是实际的 DOM 节点



问题：您想要编写可维护的测试，以使您高度确信自己的组件正在为用户工作。作为此目标的一部分，您希望测试避免包括实现细节，以使组件的重构（实现的更改而不是功能的改变）不会破坏测试并降低您和您的团队的工作效率。

组件的实现细节，例如：

- 组件内部状态
- 组件的内部方法
- 组件的生命周期方法
- 子组件
- ...



React Testing Library 是用于测试 React 组件的非常轻量级的解决方案。它在 react-dom 和 react-dom/test-utils 之上提供了轻量级的实用程序功能，从而鼓励了更好的测试实践。其主要指导原则是：

[**测试越类似于您的软件使用方式，它们就可以给您带来更多的信心。**](https://testing-library.com/docs/guiding-principles)

因此，您的测试将与实际的 DOM 节点一起使用，而不是处理渲染的 React 组件的实例。该库提供的实用程序使用户以与查询用户 DOM 相同的方式方便地查询 DOM。通过标签文本查找表单元素（就像用户看到的一样），从文本中查找链接和按钮（就像用户看到的一样）。它还公开了一种建议的方式，即通过 `data-testid` 查找元素，以作为文本内容和标签不合理或不实际的元素的情况。



该库鼓励您的应用程序更易于访问，并允许您使测试更接近于以用户将要使用的组件的方式进行使用，这使您的测试使您更有信心在实际用户使用应用程序时可以正常使用它。



该库替代了 Enzyme。尽管您可以使用 Enzyme 本身遵循这些准则，但是由于 Enzyme 提供了所有额外的实用程序（有助于测试实现细节的实用程序），因此很难实施。在 [FAQ](https://testing-library.com/docs/react-testing-library/faq) 中阅读有关此内容的更多信息。



## 准备练习模板

- [TodoMVC](https://todomvc.com/)
- [模板](https://github.com/tastejs/todomvc-app-template)



## 渲染组件

```jsx
import { render } from '@testing-library/react' // (or /dom, /vue, ...)
import App from './App.js'

test('should show login form', () => {
  render(<App />)
  const title = screen.getByText('todos')
})
```



## 查询元素

```jsx
import { render, screen } from '@testing-library/react' // (or /dom, /vue, ...)

test('should show login form', () => {
  render(<Login />)
  const input = screen.getByLabelText('Username')
  // Events and assertions...
})
```



### 手动查询

```jsx
// @testing-library/react
const { container } = render(<MyComponent />)
const foo = container.querySelector('[data-foo="bar"]')
```



### 使用查询辅助函数

使用 `render` 函数的返回值：

```jsx
import { render, screen } from '@testing-library/react'

const wrapper = render(
  <div>
    <label htmlFor="example">Example</label>
    <input id="example" />
  </div>
)

const exampleInput = wrapper.getByLabelText('Example')
```

使用 `screen` 函数（推荐）：

```jsx
import { render, screen } from '@testing-library/react'

render(
  <div>
    <label htmlFor="example">Example</label>
    <input id="example" />
  </div>
)

const exampleInput = screen.getByLabelText('Example')
```

指定 container 查询：

```jsx
import { render, screen, getByLabelText } from '@testing-library/react'

render(
  <div>
    <label htmlFor="example">Example</label>
    <input id="example" />
  </div>
)

const container = document.querySelector('#app')
const exampleInput = getByLabelText(container, 'Example')
```



### 常用查询辅助函数

- [ByRole](https://testing-library.com/docs/queries/byrole)
- [ByLabelText](https://testing-library.com/docs/queries/bylabeltext)
- [ByPlaceholderText](https://testing-library.com/docs/queries/byplaceholdertext)
- [ByText](https://testing-library.com/docs/queries/bytext)
- [ByDisplayValue](https://testing-library.com/docs/queries/bydisplayvalue)
- [ByAltText](https://testing-library.com/docs/queries/byalttext)
- [ByTitle](https://testing-library.com/docs/queries/bytitle)
- [ByTestId](https://testing-library.com/docs/queries/bytestid)



### 查询优先级

根据指导原则，您的测试应尽可能类似于用户与您的代码（组件，页面等）的交互方式。考虑到这一点，我们建议按以下优先顺序：

1、每个人均可访问的查询，这些查询反映了视觉/鼠标用户以及使用辅助技术的用户的体验

- `getByRole`：可用于查询可访问性树中公开的每个元素。使用名称选项，您可以按其可访问名称过滤返回的元素。对于几乎所有内容，这应该是您的首选。没有太多您无法做到的（如果无法做到，则可能无法访问您的UI）。通常，它将与 name 选项一起使用，例如：`getByRole('button', {name: /submit/i})`。检查角色列表。 
- `getByLabelText`：仅对表单字段真正有用，但这是用户找到这些元素的第一方法，因此它应该是您的首选。 
- `getByPlaceholderText`：占位符不能替代标签。但是，如果您仅此而已，那么它会比其他方法更好。 
- `getByText`：对表单无用，但这是用户找到大多数非交互式元素（例如 div 和 spans）的数字 1 方法。
- `getByDisplayValue`：导航带有填充值的页面时，表单元素的当前值会很有用。



2、语义查询 HTML5 和 ARIA 兼容的选择器。请注意，在浏览器和辅助技术之间，与这些属性进行交互的用户体验差异很大。

- `getByAltText`：如果您的元素是支持 `alt` 文本（`img`，`area` 和 `input`）的元素，则可以使用它来查找该元素。 
- `getByTitle`：屏幕阅读器无法始终读取title属性，默认情况下，视觉属性不可见的用户看不到该属性



3、Test IDs

- `getByTestId`：用户看不到（或听到）这些内容，因此仅在您无法按角色或文本进行匹配或没有意义（例如文本是动态的）的情况下才建议使用此方法。



### 查询辅助函数类型

**单节点查询：**

- `getBy...`：返回查询的匹配节点，如果没有元素匹配或找到多个匹配项，则抛出描述性错误（如果期望多个元素，则使用 `getAllBy` 代替）。 
- `queryBy...`：返回查询的第一个匹配节点，如果没有元素匹配，则返回 `null`。这对于声明不存在的元素很有用。如果找到多个匹配项，则会引发错误（如果可以，请使用 `queryAllBy` 代替）。 
- `findBy...`：返回一个 Promise，该 Promise 在找到与给定查询匹配的元素时进行解析。如果未找到任何元素，或者在默认的 1000ms 超时后找到一个以上的元素，则拒绝诺言。如果需要查找多个元素，请使用 `findAllBy`。



**多节点查询：**

- `getAllBy...`：返回查询的所有匹配节点的数组，如果没有元素匹配，则引发错误。 
- `queryAllBy...`：返回查询的所有匹配节点的数组，如果没有元素匹配，则返回一个空数组 `[]`。 
- `findAllBy...`：返回一个 Promise，当找到与给定查询匹配的任何元素时，该 Promise 将解析为元素数组。如果在默认的 1000ms 超时后未找到任何元素，则承诺将被拒绝。`findBy` 方法是 `getBy*` 查询和 `waitFor` 的组合。他们接受 `waitFor` 选项作为最后一个参数（即 `await screen.findByText('text', queryOptions, waitForOptions)`）。



**总结：**

| 查询类型        | 没有匹配项  | 匹配到 1 个元素 | 匹配到多个元素 | 重试(Async/Await) |
| --------------- | ----------- | --------------- | -------------- | ----------------- |
| **单节点查询**  |             |                 |                |                   |
| `getBy...`      | 抛出异常    | 返回元素        | 抛出异常       | 否                |
| `queryBy...`    | 返回 `null` | 返回元素        | 抛出异常       | 否                |
| `findBy...`     | 抛出异常    | 返回元素        | 抛出异常       | 是                |
| **多节点查询**  |             |                 |                |                   |
| `getAllBy...`   | 抛出异常    | 返回元素        | 返回节点数组   | 否                |
| `queryAllBy...` | 返回 `[]`   | 返回元素        | 返回节点元素   | 否                |
| `findAllBy...`  | 抛出异常    | 返回元素        | 返回节点元素   | 是                |

- 隐式断言和显示断言
- get 无法检查不应该存在的元素，因为它会报错；所有如果需要断言某个元素不存在，则使用 queryBy
- 什么使用使用 findBy？



### 关于查询文本匹配

大多数查询 API 都将 `TextMatch` 作为参数，这意味着该参数可以是字符串，正则表达式，或者是对于匹配项返回 `true` 以及对于不匹配项返回 `false` 的函数。

给定如下 HTML：

```html
<div>Hello World</div>
```

下面的查询将找到 div：

```js
// Matching a string:
screen.getByText('Hello World') // full string match
screen.getByText('llo Worl', { exact: false }) // substring match
screen.getByText('hello world', { exact: false }) // ignore case

// Matching a regex:
screen.getByText(/World/) // substring match
screen.getByText(/world/i) // substring match, ignore case
screen.getByText(/^hello world$/i) // full string match, ignore case
screen.getByText(/Hello W?oRlD/i) // substring match, ignore case, searches for "hello world" or "hello orld"

// Matching with a custom function:
screen.getByText((content, element) => content.startsWith('Hello'))
```

下面的查询找不到 div：

```js
// full string does not match
screen.getByText('Goodbye World')

// case-sensitive regex with different case
screen.getByText(/hello world/)

// function looking for a span when it's actually a div:
screen.getByText((content, element) => {
  return element.tagName.toLowerCase() === 'span' && content.startsWith('Hello')
})
```



### 调试查询

使用 `screen.debug()` 可以打印查询到的元素 DOM，本质上是 `console.log(prettyDOM())` 的快捷方式。

```js
import { screen } from '@testing-library/dom'

document.body.innerHTML = `
  <button>test</button>
  <span>multi-test</span>
  <div>multi-test</div>
`

// debug document
screen.debug()
// debug single element
screen.debug(screen.getByText('test'))
// debug multiple elements
screen.debug(screen.getAllByText('multi-test'))
```



### Testing Playground

- [testing-playground.com](https://testing-playground.com/)
- `screen.logTestingPlaygroundURL()`
- [浏览器插件](https://chrome.google.com/webstore/detail/testing-playground/hejbmebodbijjdhflfknehhcgaklhano/related)



## 扩展的断言匹配器

参考：https://github.com/testing-library/jest-dom。

- `toBeDisabled`
- `toBeEnabled`
- `toBeEmpty`
- `toBeEmptyDOMElement`
- `toBeInTheDocument`
- `toBeInvalid`
- `toBeRequired`
- `toBeValid`
- `toBeVisible`
- `toContainElement`
- `toContainHTML`
- `toHaveAttribute`
- `toHaveClass`
- `toHaveFocus`
- `toHaveFormValues`
- `toHaveStyle`
- `toHaveTextContent`
- `toHaveValue`
- `toHaveDisplayValue`
- `toBeChecked`
- `toBePartiallyChecked`
- `toHaveDescription`



## 触发 DOM 事件

基本语法：

```js
fireEvent(node: HTMLElement, event: Event)
```

触发 DOM 事件：

```js
// <button>Submit</button>
fireEvent(
  getByText(container, 'Submit'),
  new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
  })
)
```

触发 DOM 事件的快捷方式：

```js
fireEvent[eventName](node: HTMLElement, eventProperties: Object)
```

[快捷方式完整列表参考](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js)。



## 扩展的 user-event

`user-event` 是测试库的配套库，它提供了比内置 `fireEvent` 方法更高级的浏览器交互模拟。

- `click(element, eventInit, options)`
- `dblClick(element, eventInit, options)`
- `type(element, text, [options])`
- `keyboard(text, options)`
- `upload(element, file, [{ clickInit, changeInit }], [options])`
- `clear(element)`
- `selectOptions(element, values)`
- `deselectOptions(element, values)`
- `tab({shift, focusTrap})`
- `hover(element)`
- `unhover(element)`
- `paste(element, text, eventInit, options)`
- `specialChars`

