

[TOC]

# API

## Array.prototype.sort()

**sort()** 方法用原地算法对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的。[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

**参数：**

**compareFunction（可选）：**

用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。

**返回值：**

排序后的数组。请注意，数组已原地排序，并且不进行复制。

**备注：**

如果指明了 **compareFunction** ，那么数组会按照调用该函数的返回值排序。即 a 和 b 是两个将要被比较的元素，**a代表数组中后一个元素，b代表前一个元素**：

- 如果 **compareFunction(a, b)** 小于 0 ，那么 a 会被排列到 b 之前；

- 如果 **compareFunction(a, b)** 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；

- 如果 **compareFunction(a, b)** 大于 0 ， b 会被排列到 a 之前。
- **compareFunction(a, b)** 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。



## Array.from 和[...]的区别

> Array.from(arr) 和[...arr] 都可以将类数组arr转换成数组

- 什么叫类数组

  - 有数字索引
  - 有长度length
  - 是个对象
  - 能被迭代

- 如果arr只有索引和长度，并且是对象，所以可以被Array.from转换成数组的，但是[...arr]方法，就必须可以被迭代

  ```js
  let obj = {'0': 1,'1': 2,'2': 3,length: 3}
  let arr = Array.from(obj)
  console.log(arr)
  let arr1= [...obj]
  console.log(arr1)
  ```

- 上面的obj 因为不能被迭代 所以在进行[...obj]转换的时候就会报错`object is not iterable`,所以我们如果需要在obj上进行...运算，还需要在上面增加一个属性[Symbol.iterator]

```js
let obj = {
    '0': 1,
    '1': 2,
    '2': 3,
    length: 4,
    [Symbol.iterator]: function(){
        let index = 0
        let next = () => {
            return {
                value: this[index],
                done: this.length == ++ index
            }
        }
        return {
            next
        }
    }
}
let arr = Array.from(obj)
console.log(arr)
let arr1= [...obj]
console.log(arr1)
```
