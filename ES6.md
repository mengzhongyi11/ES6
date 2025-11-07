# 一、let和const

### let

**用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效，即let声明的是一个块作用域内的变量。**

**特点：**

1.不存在变量提升。
2.暂时性死区——只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
3.不允许重复声明。
4.块级作用域——被{}包裹，外部不能访问内部。
应用案例与分析：
// 使用var

```js
for (var i = 0; i < 5; i++) {
setTimeout(function () {
console.log(i);
});
} // => 5 5 5 5 5
```

// 使用let

```js
for (let i = 0; i < 5; i++) {
setTimeout(function () {
console.log(i);
});
} // => 0 1 2 3 4
```

 var是全局/函数作用域，整个循环只存在1个i，异步回调捕获的是同一个变量，循环结束后i已变最终值。

 let是块级作用域，每次循环会创建独立的i（绑定当前迭代值），异步回调捕获的是各自迭代的独立变量，不受后续循环影响。



# 二、数组的扩展

### 扩展运算符

也可以将对象转换为数组

```js
const kuo = […ob（对象）]
```

**扩展运算符（spread）是三个点（...），将一个数组转为用逗号分隔的参数序列**

应用场景：

#### 1 复制数组

```js
const a1 = [1, 2];

const a2 = [...a1];
```

#### 2 合并数组

```js
const arr1 = ['1', '2'];

const arr2 = ['c', {a:1} ];
```

#### // 3.ES6 的合并数组

```js
[...arr1, ...arr2]
```

注：这两种方法都是浅拷贝，使用的时候需要注意。将字符串转化为数组

使用扩展运算符能够正确识别四个字节的 Unicode 字符。凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写。

```js
[...'xuxi']

// [ "x", "u", "x", "i" ]
```





#### 4 实现了 Iterator 接口的对象

```js
let nodeList = document.querySelectorAll('div');

let arr = [...nodeList];
```

上面代码中，querySelectorAll方法返回的是一个NodeList对象。它不是数组，而是一个类似数组的对象。扩展运算符可以将其转为真正的数组，原因就在于NodeList对象实现了 Iterator 。

##### 核心意思：

扩展运算符（...）能处理实现了 Iterator 接口的对象，NodeList 符合这个条件，所以可通过...转为数组。
扩展运算符的底层逻辑是调用对象的 Iterator 接口，依次遍历其元素并收集，最终组装成新数组——NodeList 虽不是数组，但有 Iterator 提供的遍历能力，因此能被...识别并转换
Iterator（迭代器） 是 JS 中遍历可迭代对象（如数组、Set、Map 等）的接口，本质是带  next()  方法的对象。
调用  next()  会返回  { value: 当前值, done: 是否遍历结束 } ，通过循环调用可依次获取所有元素，是  for...of  循环的底层实现。

#### Array.from()

**Array.from方法用于将类对象转为真正的数组：类似数组的对象和可遍历的对象（包括 ES6 新增的数据结构 Set 和 Map）。**

**实际应用中我们更多的是将Array.from用于DOM 操作返回的 NodeList 集合，以及函数内部的arguments对象。**

**// NodeList对象**

```js
let nodeList = document.querySelectorAll('p')

let arr = Array.from(nodeList)
```

**// arguments对象**

```js
function say() {

let args = Array.from(arguments);

}
```

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```js
Array.from([1, 2, 4], (x) => x + 1)

// [2, 3, 5]

Array.of()
```

#### Array.of

**Array.of方法用于将一组值，转换为数组。Array.of基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一。**

```js
Array.of() // []

Array.of(undefined) // [undefined]

Array.of(2) // [21]

Array.of(21, 2) // [21, 2]
```



#### **数组实例方法includes()**

**Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值。该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。**

```js
[1, 4, 3].includes(2) // true

[1, 2, 4].includes(3) // false

[1, 5, NaN, 6].includes(NaN) // true


```



# 三、对象的扩展

## 对象的扩展运算符

**对象的扩展运算符（...）用于取出参数对象的所有可遍历属性，拷贝到当前对象之中，等同于使用Object.assign()方法。**

```js
let a = {

w: 'xu', y: 'xi'

}
let b = {

name: '12'}
let ab = {

...a, ...b

};

// 等同于

let ab = Object.assign({}, a, b);

Object.is()
```

用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致;不同之处只有两个：一是+0不等于-0，二是NaN等于自身。

```js
+0 === -0 //true

NaN === NaN // false

Object.is(+0, -0) // false

Object.is(NaN, NaN) // true

Object.assign()
```

用于对象的合并，将源对象的所有可枚举属性，复制到目标对象; 如果只有一个参数，Object.assign会直接返回该参数; 由于undefined和null无法转成对象，所以如果它们作为参数，就会报错; 其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。

#### // 合并对象

```js
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };

const source2 = { c: 3 };

Object.assign(target, source1, source2);

target // {a:1, b:2, c:3}

// 非对象和字符串的类型将忽略

const a1 = '123';

const a2 = true;

const a3 = 10;

const obj = Object.assign({}, a1, a2, a3);

console.log(obj); // { "0": "1", "1": "2", "2": "3" }


```



#### Object.assign

Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

对于嵌套的对象，遇到同名属性，Object.assign的处理方法是替换，而不是添加

Object.assign可以用来处理数组，但是会把数组视为对象。

```js
Object.assign([1, 2, 3], [4, 5])

// [4, 5, 3]
```

Object.assign只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

```js
const a = {

get num() { return 1 }

};

const target = {};

Object.assign(target, a)

// { num: 1 }
```

应用场景：

为对象添加属性和方法
克隆/合并对象
为属性指定默认值



#### Object.keys()

**返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键名。**

```
const obj = { 100: '1', 2: '2', 7: '3' };

Object.values(obj)

// ["100", "2", "7"]

Object.values()
```

返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值。注意：返回数组的成员顺序：如果属性名为数值的属性，是按照数值大小，从小到大遍历的。

```js
const obj = { 100: '1', 2: '2', 7: '3' };

Object.values(obj)

// ["2", "3", "1"]
```



# 四、set和map数据结构

#### set

**ES6提供了新的数据结构Set,类似于数组，但是成员的值都是唯一的，没有重复的值。Set本身是一个构造函数，用来生成Set数据结构。**

**实例属性和方法：**

**add(value)：添加某个值，返回Set结构本身。**
**delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。**
**has(value)：返回一个布尔值，表示该值是否为Set的成员。**
**clear()：清除所有成员，没有返回值。**
**s.add(1).add(3).add(3);**

```js
// 注意3被加入了两次

s.size // 2

s.has(1) // true

s.has(2) // false

s.delete(3);

s.has(3) // false
```

**可进行遍历操作：**

**keys()：返回键名的遍历器**
**values()：返回键值的遍历器**
**entries()：返回键值对的遍历器**
**forEach()：使用回调函数遍历每个成员**
**Set的遍历顺序就是插入顺序，这个特性有时非常有用，比如使用Set保存一个回调函数列表，调用时就能保证按照添加顺序调用。**

应用场景：

```js
// 数组去重

let arr = [1，2，2，3];

let unique = [...new Set(arr)];

// or

function dedupe(array) {

return Array.from(new Set(array));

}

let a = new Set([1, 2, 3]);

let b = new Set([4, 3, 2]);

// 并集

let union = new Set([...a, ...b]);

// Set {1, 2, 3, 4}

// 交集

let intersect = new Set([...a].filter(x => b.has(x)));

// set {2, 3}

// 差集

let difference = new Set([...a].filter(x => !b.has(x)));

// Set {1}


```



#### map

**类似于对象，也是键值对的集合，各种类型的值（包括对象）都可以当作键。Map结构提供了“值与值”的对应，是一种更完善的Hash结构实现。**

**实例属性和方法:**

size属性: 返回Map结构的成员总数。

set(key, value): set方法设置key所对应的键值，然后返回整个Map结构。如果key已经有值，则键值会被更新，否则就新生成该键,set方法返回的是Map本身，因此可以采用链式写法。

get(key) : get方法读取key对应的键值，如果找不到key，返回undefined。

has(key) : has方法返回一个布尔值，表示某个键是否在Map数据结构中。

delete(key) : delete方法删除某个键，返回true。如果删除失败，返回false。

clear() : clear方法清除所有成员，没有返回值。
遍历方法和set类似，Map结构转为数组结构，比较快速的方法是结合使用扩展运算符（...）：

```js
let map = new Map([

[1, 'one'],

[2, 'two'],

[3, 'three'],

]);

[...map.keys()]

// [1, 2, 3]

[...map.values()]

// ['one', 'two', 'three']

[...map.entries()]

// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]

// [[1,'one'], [2, 'two'], [3, 'three']]
```

##### 数组转map：

```js
new Map([[true, 7], [{foo: 3}, ['abc']]])// Map {true => 7, Object {foo: 3} => ['abc']}
```

##### Map转为对象:

```js
function strMapToObj(strMap) {

let obj = Object.create(null);

for (let [k,v] of strMap) {

obj[k] = v;

}

return obj;

}

let myMap = new Map().set('yes', true).set('no', false);

strMapToObj(myMap)

// { yes: true, no: false }

对象转为Map：

function objToStrMap(obj) {

let strMap = new Map();

for (let k of Object.keys(obj)) {

strMap.set(k, obj[k]);

}

return strMap;

}

objToStrMap({yes: true, no: false})

// [ [ 'yes', true ], [ 'no', false ] ]
```



1. #### Array.prototype.find()

- 作用：返回数组中第一个满足回调函数条件的元素，无则返回  undefined 。

- 语法： arr.find((item, index, arr) => 条件) （item=当前元素，index=索引，arr=原数组）

```JS
 const arr = [10, 20, 30, 40];
const result = arr.find(item => item > 25); // 30（第一个大于25的元素）
```




2. #### Array.prototype.findIndex()
 
 ```JS
 const arr = [10, 20, 30, 40];
 const index = arr.findIndex(item => item > 25); // 2（元素30的索引）
 ```
 
 
- 作用：返回数组中第一个满足回调函数条件的元素的索引，无则返回  -1 。
- 语法与  find  一致，仅返回值不同



1. #### for...in

- 作用：遍历对象的可枚举属性（包括原型链上的属性），数组中会遍历索引（字符串类型）。

- 语法： for (let key in obj/arr) { ... } （key 是属性名/数组索引）。

  ```JS
  const obj = { a: 1, b: 2 };
  for (let key in obj) {
  if (obj.hasOwnProperty(key)) { // 过滤原型属性
    console.log(key, obj[key]); // a 1, b 2
  }
  }
  ```

  

- 注意：需用  hasOwnProperty  过滤原型属性，避免遍历到非自身属性。



2. #### for...of

- 作用：遍历可迭代对象的元素值（如数组、Set、Map、字符串等），不关心索引/属性名。

- 语法： for (let value of iterable) { ... } （value 是当前元素值）。

  ```JS
  - const arr = [1, 2, 3];
    for (let value of arr) {
    console.log(value); // 1, 2, 3（直接取元素值）
    }
  - 
  ```

  关键：仅支持实现 Iterator 接口的对象（如数组、NodeList，不支持普通对象）。



1. #### entries()

- 作用：返回包含「键值对数组」的迭代器，数组中键是索引，对象中键是属性名。

- 语法： iterable.entries() ，遍历结果为  [key, value]  形式。
 
 ```JS
  const arr = ['a', 'b'];
 for (let [index, value] of arr.entries()) {
 console.log(index, value); // 0 'a'、1 'b'
 }
 ```

2. #### keys()

- 作用：返回包含「所有键」的迭代器，数组中键是索引（数字），对象中是属性名（字符串）。

- 语法： iterable.keys() ，遍历结果为单个键值。
 
 ```JS
  const obj = { x: 1, y: 2 };
 for (let key of Object.keys(obj)) { // 对象需用 Object.keys()
 console.log(key); // 'x'、'y'
 }
 ```
 
 

3. #### values()

- 作用：返回包含「所有值」的迭代器，直接获取每个元素/属性对应的值。

- 语法： iterable.values() ，遍历结果为单个值。const set = new Set([3, 4]);

  ```JS
  for (let value of set.values()) {
  console.log(value); // 3、4
  }
  ```

  



1. #### 数组解构（按顺序提取）

- 语法： let [a, b, c] = 数组 ，变量顺序对应数组索引。

- 支持默认值、跳过元素、剩余参数（ ... ）。
 
 ```JS
 const [x, y] = [10, 20]; // x=10，y=20
 const [a, , b] = [1, 2, 3]; // 跳过2，a=1，b=3
 const [c, d = 5] = [3]; // d取默认值5
 const [e, ...rest] = [4,5,6]; // rest=[5,6]（剩余元素）
 ```
 
 解构时  [e, ...rest]  对应原数组  [4,5,6] ： e  匹配索引 0 的元素， ...rest  收集剩余所有元素（索引 1 及以后），所以  e=4 、 rest=[5,6] 。


2. #### 对象解构（按属性名提取）

- 语法： let { key1, key2 } = 对象 ，变量名需与属性名一致。

  ```JS
  const { name, age } = { name: "Tom", age: 20 }; // name="Tom"，age=20
  const { id: userId = 1 } = { name: "Jerry" }; // 重命名为userId，默认值1
  const { address: { city } } = { address: { city: "Beijing" } }; // 嵌套解构，city="Beijing"
  const { name, age } = { name: "Tom", age: 20 }; // name="Tom"，age=20
  const { id: userId = 1 } = { name: "Jerry" }; // 重命名为userId，默认值1
  const { address: { city } } = { address: { city: "Beijing" } }; // 嵌套解构，city="Beijing"
  ```

  

- 支持重命名、默认值、嵌套解构

在funtion方法中

```JS
const FN = {
   age,
   name
}
funtion({age,name})
```

