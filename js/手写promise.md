[TOC]

# 手写promise

```js
// 状态集合（定义为常量，方便代码复用和编辑器提示）
const PENDING = "pending"; // 等待
const FULFILLED = "fulfilled"; // 成功
const REJECTED = "rejected"; // 失败

/Promise 类
class MyPromise {
  constructor(executor) {
    try {
      executor(this.resolve, this.reject);
    } catch (e) {
      // 捕获执行器中的错误，执行 reject
      this.reject(e);
    }
  }

  // promise 状态
  status = PENDING;
  // 成功之后的值
  value = undefined;
  // 失败之后的原因
  reason = undefined;
  // 成功回调
  successCallback = [];
  // 失败回调
  failCallback = [];

  // 使用箭头函数，确保内部 this 指向当前 promise 实例而不是 window / undefined
  resolve = (value) => {
    // 如果状态不是等待，阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为成功
    this.status = FULFILLED;

    // 保存成功之后的值
    this.value = value;
    // 判断成功回调是否存在，如果存在就依次执行，并把值传递给回调
    // this.successCallback && this.successCallback()
    while (this.successCallback.length) this.successCallback.shift()();
  };

  reject = (reason) => {
    // 如果状态不是等待，阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为失败
    this.status = REJECTED;

    // 保存失败之后的原因
    this.reason = reason;
    // 判断成功回调是否存在，如果存在就依次执行，并把值传递给回调
    // this.failCallback && this.failCallback()
    while (this.failCallback.length) this.failCallback.shift()();
  };

  then(successCallback, failCallback) {
    // 处理 then 方法没有传参数的情况下，把 value/reason 传递到下一个有参数的 then 方法中
    successCallback = successCallback ? successCallback : (value) => value;
    failCallback = failCallback
      ? failCallback
      : (reason) => {
          throw reason;
        };
    // 为了 then 方法链式调用，返回一个新的 promise 对象
    const newPromise = new MyPromise((resolve, reject) => {
      // 判断状态
      // 成功状态
      if (this.status === FULFILLED) {
        // 添加异步回调，确保执行器中能正确获取到 newPromise 对象
        setTimeout(() => {
          try {
            // 将上一个 promise 对象成功回调的返回值，传递到下一个 promise 对象的成功回调接收的参数中
            const res = successCallback(this.value);
            // 解析返回值
            return resolvePromise(newPromise, res, resolve, reject);
          } catch (e) {
            // 捕获 successCallback 的错误，执行 reject
            reject(e);
          }
        }, 0);
      }
      // 失败状态
      else if (this.status === REJECTED) {
        // 添加异步回调，确保执行器中能正确获取到 newPromise 对象
        setTimeout(() => {
          try {
            // 将上一个 promise 对象成功回调的返回值，传递到下一个 promise 对象的成功回调接收的参数中
            const res = failCallback(this.reason);
            // 解析返回值
            return resolvePromise(newPromise, res, resolve, reject);
          } catch (e) {
            // 捕获 failCallback 的错误，执行 reject
            reject(e);
          }
        }, 0);
      }
      // 仍在等待状态，证明是异步执行 resolve/reject
      else {
        // 将成功回调和失败回调存储起来
        this.successCallback.push(() => {
          // 添加异步回调，确保执行器中能正确获取到 newPromise 对象
          setTimeout(() => {
            try {
              // 将上一个 promise 对象成功回调的返回值，传递到下一个 promise 对象的成功回调接收的参数中
              const res = successCallback(this.value);
              // 解析返回值
              return resolvePromise(newPromise, res, resolve, reject);
            } catch (e) {
              // 捕获 successCallback 的错误，执行 reject
              reject(e);
            }
          }, 0);
        });
        this.failCallback.push(() => {
          // 添加异步回调，确保执行器中能正确获取到 newPromise 对象
          setTimeout(() => {
            try {
              // 将上一个 promise 对象成功回调的返回值，传递到下一个 promise 对象的成功回调接收的参数中
              const res = failCallback(this.reason);
              // 解析返回值
              return resolvePromise(newPromise, res, resolve, reject);
            } catch (e) {
              // 捕获 failCallback 的错误，执行 reject
              reject(e);
            }
          }, 0);
        });
      }
    });
    return newPromise;
  }

  catch(failCallback) {
    return this.then(null, failCallback);
  }

  finally(callback) {
    // 不管成功还是失败都执行 callback， return this.then() 使后续可以链式调用
    return this.then(
      (value) => {
        // 如果 callback() 返回的是 promise 对象，等待该 promise 对象成功或者失败才执行后续 then 操作，并且将 value/reason 传递到下一个 then 回调中
        return MyPromise.resolve(callback()).then(() => value);
      },
      (reason) => {
        return MyPromise.resolve(callback()).then(() => {
          throw reason;
        });
      }
    );
  }

  static all(array) {
    const results = [];
    let index = 0; // 计数器
    return new Promise((resolve, reject) => {
      function addData(key, value) {
        results[key] = value;
        index++;
        // 所有 promise 对象都成功，才执行 all 方法的 resolve
        if (index === array.length) {
          resolve(results);
        }
      }
      for (let i = 0, len = array.length; i < len; i++) {
        const current = array[i];
        if (current instanceof MyPromise) {
          current.then(
            (value) => addData(i, value),
            (reason) => reject(reason) // 如果有一个 promise 对象失败，那么执行 all 方法的 reject
          );
        } else {
          addData(i, current);
        }
      }
    });
  }

  static resolve(value) {
    // 如果是 promise 对象，直接原样返回
    if (value instanceof MyPromise) return value;
    // 如果是普通值，包装成立即执行 resolve 的 promise 对象
    return new MyPromise((resolve) => resolve(value));
  }
}

/*  判断 value 的值是普通值还是 promise 对象
          如果是普通值，直接调用resolve 
          如果是 promise 对象，查看 promsie 对象返回的结果 
          再根据 promise 对象返回的结果 决定调用 resolve 还是调用 reject 
      */
function resolvePromise(promise, value, resolve, reject) {
  // 防止成功回调返回的值是 then 方法返回的新的 promise 对象 ，造成循环调用
  if (promise === value) {
    // TypeError: Chaining cycle detected for promise #<Promise>
    return reject(
      new TypeError("Chaining cycle detected for promise #<Promise>")
    );
  }
  // promise 对象
  if (value instanceof MyPromise) {
    value.then(resolve, reject);
  }
  // 普通值
  else {
    resolve(value);
  }
}
```

