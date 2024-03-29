[TOC]

# js笔记

## 后端返回图片文件流，前端如何显示到页面上

方法一、使用 URL.createObjectURL 将 Blob 对象转化成图片 url：

```js
xhr.responseType = "blob";  // xhr 对象的 responseType 需要设置为 blob
// 将文件流转化成 Blob 对象
var blob = new Blob([res.response]);
// 将 Blob 对象转化成图片 url
var url = window.URL.createObjectURL(blob);
var guomiQrCodeImg = document.getElementById("guomiQrCode");
guomiQrCodeImg.src = url;
```



方法二、使用 FileReader 读取 Blob 对象，转化成 base64 字符串：

```js
xhr.responseType = "blob";  // xhr 对象的 responseType 需要设置为 blob        
// 将 blob 转化为 base64 形式
var reader = new FileReader();
reader.readAsDataURL(res.response);
reader.onload = function () {
   var base64Data = reader.result; // 这里 base64Data 就是请求到的图片的 base64 字符串
   var guomiQrCodeImg = document.getElementById("guomiQrCode");
   guomiQrCodeImg.src = base64Data;
};
```



## while 和 forEach 取出函数执行区别

**示例:** 

```js
const resolves = [];
resolves.push(() => console.log(1));
resolves.push(() => console.log(2));
resolves.push(() => console.log(3));

while (resolves.length > 0) {
  const resolve = resolves.shift();
  resolve();
}

resolves.forEach((resolve) => resolve());
```

**区别：**

`forEach`遍历时候每次仅仅取出函数执行，而 `while` 可以在每次取出函数执行之后清除数组中该函数的引用以便垃圾回收，因此 `while` 的性能比 `forEach` 更好。`forEach` 若要优化，可以在其后增加 `resolves.length = 0;`



## for await of 循环

异步迭代器(for-await-of)：循环等待每个Promise对象变为resolved状态才进入下一步。

for of 循环，用于**遍历同步的 Iterator 接口**。
for await of 循环，虽然可以用于同步遍历器，但是**主要用于遍历异步的 Iterator 接口**。

```js
(async () => {
   for await (const id of selectedIds) {
    	await updateQuestionService(id, { isDeleted: false }) // 异步操作
   }
})()
```

