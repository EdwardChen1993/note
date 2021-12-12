[TOC]

# js笔记

## 后端返回图片文件流，前端如何显示到页面上

方法一，使用 URL.createObjectURL 将 Blob 对象转化成图片 url：

```js
xhr.responseType = "blob";  // xhr 对象的 responseType 需要设置为 blob
// 将文件流转化成 Blob 对象
var blob = new Blob([res.response]);
// 将 Blob 对象转化成图片 url
var url = window.URL.createObjectURL(blob);
var guomiQrCodeImg = document.getElementById("guomiQrCode");
guomiQrCodeImg.src = url;
```



方法二，使用 FileReader 读取 Blob 对象，转化成 base64 字符串：

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

