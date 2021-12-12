[TOC]

# jQuery

## $.ajax(url,[settings])

### settings

**xhrFields**

设置本地 xhr 对象的“名-值”映射。例如，可以在需要时设置 `withCredentials` 为 `true` 而执行跨域名请求。

```js
$.ajax(url, {
	xhrFields: {
    {
  		withCredentials: true,
			responseType: "blob" // 指定响应中包含的数据类型
		}
  }
})
```

