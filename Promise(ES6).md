# Promise(ES6)

## Promise 详细解析

1. #### 核心定位

Promise 是 ES6 引入的异步编程规范，用于解决回调嵌套（回调地狱）问题，让异步逻辑更清晰、可维护。

2. #### 核心特性（状态不可逆）

- 三种状态：
-  pending ：初始状态，异步操作未完成（如请求中、定时器未触发）。
-  fulfilled ：异步操作成功（如请求返回数据），状态不可逆。
-  rejected ：异步操作失败（如请求报错、文件读取失败），状态不可逆。
- 状态一旦从  pending  转为  fulfilled  或  rejected ，就会固定，无法再变更。

3. #### 基本用法

通过  new Promise((resolve, reject) => { ... })  创建实例，接收一个执行器函数（同步执行），参数为两个函数：

-  resolve(data) ：异步成功时调用，将状态转为  fulfilled ，并传递成功结果  data 。
-  reject(error) ：异步失败时调用，将状态转为  rejected ，并传递失败原因  error 。

javascript

```js
// 示例：模拟异步请求
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("请求成功的数据"); // 成功：状态→fulfilled，传递数据
    } else {
      reject(new Error("网络错误")); // 失败：状态→rejected，传递错误
    }
  }, 1000);
});
```




4. #### 关键方法（处理异步结果）

-  .then(onFulfilled, onRejected) ：
- 接收两个可选回调，分别处理  fulfilled  和  rejected  状态。
- 返回一个新的 Promise，支持链式调用（解决回调地狱的核心）。
-  .catch(onRejected) ：
- 专门处理  rejected  状态（等价于  .then(null, onRejected) ），更简洁。
-  .finally(onFinally) ：
- 无论状态成功/失败，都会执行（如关闭加载动画），不接收状态数据。

javascript

```js
// 链式调用示例
fetchData
  .then((data) => {
    console.log("成功：", data); // 输出"请求成功的数据"
    return data + "（处理后）"; // 传递给下一个then
  })
  .then((processedData) => {
    console.log("二次处理：", processedData);
  })
  .catch((error) => {
    console.log("失败：", error.message); // 捕获所有上游错误
  })
  .finally(() => {
    console.log("异步操作结束（无论成败）");
  });
```




5. #### 常用静态方法（批量处理异步）

-  Promise.resolve(data) ：快速创建一个  fulfilled  状态的 Promise，直接传递  data 。
-  Promise.reject(error) ：快速创建一个  rejected  状态的 Promise，直接传递  error 。
-  Promise.all([p1, p2, p3]) ：
- 接收 Promise 数组，全部成功才成功（返回结果数组），一个失败则整体失败（返回第一个失败的错误）。
-  Promise.race([p1, p2, p3]) ：
- 接收 Promise 数组，哪个先完成就取哪个结果（无论成功/失败）。
