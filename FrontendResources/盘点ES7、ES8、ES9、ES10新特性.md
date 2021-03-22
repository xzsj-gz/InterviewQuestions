前言
------

从 ECMAScript 2016（ES7）开始，版本发布变得更加频繁，每年发布一个新版本，好在每次版本的更新内容并不多，本文会细说这些新特性，尽可能和旧知识相关联，帮你迅速上手这些特性。
![](https://img-blog.csdnimg.cn/20210319161418140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1haU0pfRnJvbnQ=,size_16,color_FFFFFF,t_70#pic_center)


ES7新特性
------

### 1.Array.prototype.includes()方法

在 ES6 中我们有 String.prototype.includes() 可以查询给定字符串是否包含一个字符，而在 ES7 中，我们在数组中也可以用 Array.prototype.includes 方法来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回true，否则返回false。

```javascript
const arr = [1, 3, 5, 2, '8', NaN, -0]
arr.includes(1) // true
arr.includes(1, 2) // false 该方法的第二个参数表示搜索的起始位置，默认为0
arr.includes('1') // false
arr.includes(NaN) // true
arr.includes(+0) // true 
```

在ES7之前想判断数组中是否包含一个元素，有如下两种方法,但都不如includes来得直观：

*   indexOf()

indexOf()方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

```javascript
if (arr.indexOf(el) !== -1) {
  // ...
} 
```

不过这种方法有两个缺点，一是不够语义化，要先找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判。

```javascript
[NaN].indexOf(NaN)// -1 
```

*   find() 和 findIndex()

数组实例的find方法，用于找出第一个符合条件的数组成员。另外，这两个方法都可以发现NaN，弥补了数组的indexOf方法的不足。

```javascript
[1, 4, -5, 10].find((n) => n < 0) // -5
[1, 5, 10, 15].findIndex(function(value) {
  return value > 9;
}) // 2
[NaN].findIndex(y => Object.is(NaN, y)) // 0 
```

Array.prototype.includes()的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/e134e5adf26d10d2a0ece24e9651b26f.png)

### 2.求幂运算符**

在ES7中引入了指数运算符，具有与Math.pow()等效的计算结果

```javascript
console.log(2**10);// 输出1024zhicc
console.log(Math.pow(2, 10)) // 输出1024 
```

求幂运算符的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/064873d8f3a759932fd04ab291ff5cdf.png)

ES8新特性
------

### 1.Async/Await

我们都知道使用Promise能很好地解决回调地狱的问题，但如果处理流程比较复杂的话，那么整段代码将充斥着then，语义化不明显，代码不能很好地表示执行流程，那有没有比Promise更优雅的异步方式呢？

假如有这样一个使用场景：需要先请求a链接，等返回信息之后，再请求b链接的另外一个资源。下面代码展示的是使用fetch来实现这样的需求，fetch被定义在window对象中，它返回的是一个Promise对象

```javascript
fetch('https://blog.csdn.net/')
  .then(response => {
    console.log(response)
    return fetch('https://juejin.im/')
  })
  .then(response => {
    console.log(response)
  })
  .catch(error => {
    console.log(error)
  }) 
```

虽然上述代码可以实现这个需求，但语义化不明显，代码不能很好地表示执行流程。基于这个原因，ES8引入了async/await，这是JavaScript异步编程的一个重大改进，提供了在不阻塞主线程的情况下使用同步代码实现异步访问资源的能力，并且使得代码逻辑更加清晰。

```javascript
async function foo () {
  try {
    let response1 = await fetch('https://blog.csdn.net/')
    console.log(response1)
    let response2 = await fetch('https://juejin.im/')
    console.log(response2)
  } catch (err) {
    console.error(err)
  }
}
foo() 
```

通过上面代码，你会发现整个异步处理的逻辑都是使用同步代码的方式来实现的，而且还支持try catch来捕获异常，这感觉就在写同步代码，所以是非常符合人的线性思维的。需要强调的是，**await 不可以脱离 async 单独使用**，await 后面一定是Promise 对象，如果不是会自动包装成Promise对象。

根据MDN定义，**async是一个通过异步执行并隐式返回Promise作为结果的函数**。

```javascript
async function foo () {
  return '浪里行舟'
}
foo().then(val => {
  console.log(val) // 浪里行舟
}) 
```

上述代码，我们可以看到调用async 声明的foo 函数返回了一个Promise对象，等价于下面代码：

```javascript
async function foo () {
  return Promise.resolve('浪里行舟')
}
foo().then(val => {
  console.log(val) // 浪里行舟
}) 
```

Async/Await的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/ea69a414c793e67e17302acb373c99a7.png)

### 2.Object.values()，Object.entries()

ES5 引入了Object.keys方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。ES8引入了跟Object.keys配套的Object.values和Object.entries，作为遍历一个对象的补充手段，供for...of循环使用。

Object.values方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。

```javascript
const obj = { foo: 'bar', baz: 42 };
Object.values(obj) // ["bar", 42]
const obj = { 100: 'a', 2: 'b', 7: 'c' };
Object.values(obj) // ["b", "c", "a"] 
```

需要注意的是，**如果属性名为数值的属性，是按照数值大小，从小到大遍历的**，因此返回的顺序是b、c、a。

Object.entries()方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。这个特性我们后面介绍ES10的Object.fromEntries()还会再提到。

```javascript
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj) // [ ["foo", "bar"], ["baz", 42] ]
const obj = { 10: 'xxx', 1: 'yyy', 3: 'zzz' };
Object.entries(obj); // [['1', 'yyy'], ['3', 'zzz'], ['10': 'xxx']] 
```

Object.values()与Object.entries()兼容性一致，下面以Object.values()为例：
![](https://img-blog.csdnimg.cn/img_convert/45a21aa23f16de93e757af22b607a5d8.png)

### 3.String padding

在ES8中String 新增了两个实例函数 String.prototype.padStart 和 String.prototype.padEnd，允许将空字符串或其他字符串添加到原始字符串的开头或结尾。我们先看下使用语法：

```javascript
String.padStart(targetLength,[padString]) 
```

*   targetLength(必填):当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
*   padString(可选):填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断，此参数的缺省值为 " "。

```javascript
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab' 
```

有时候我们处理日期、金额的时候经常要格式化，这个特性就派上用场：

```javascript
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12" 
```

String padding的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/08d891f29834340e7ff11c4ace148735.png)

### 4.Object.getOwnPropertyDescriptors()

ES5 的Object.getOwnPropertyDescriptor()方法会返回某个对象属性的描述对象（descriptor）。ES8 引入了Object.getOwnPropertyDescriptors()方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
  name: '浪里行舟',
  get bar () {
    return 'abc'
  }
}
console.log(Object.getOwnPropertyDescriptors(obj)) 
```

得到如下结果：
![](https://img-blog.csdnimg.cn/img_convert/c6c13fbefe1eae4405576ef4a02a4169.png)
该方法的引入目的，主要是为了解决Object.assign()无法正确拷贝get属性和set属性的问题。我们来看个例子：

```javascript
const source = {
  set foo (value) {
    console.log(value)
  },
  get bar () {
    return '浪里行舟'
  }
}
const target1 = {}
Object.assign(target1, source)
console.log(Object.getOwnPropertyDescriptor(target1, 'foo')) 
```

返回如下结果：
![](https://img-blog.csdnimg.cn/img_convert/1c75c88b5dfd2db0419c5ccdfdd67112.png)
上面代码中，source对象的foo属性的值是一个赋值函数，Object.assign方法将这个属性拷贝给target1对象，结果该属性的值变成了undefined。这是因为**Object.assign方法总是拷贝一个属性的值，而不会拷贝它背后的赋值方法或取值方法**。

这时，Object.getOwnPropertyDescriptors()方法配合Object.defineProperties()方法，就可以实现正确拷贝。

```javascript
const source = {
  set foo (value) {
    console.log(value)
  },
  get bar () {
    return '浪里行舟'
  }
}
const target2 = {}
Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source))
console.log(Object.getOwnPropertyDescriptor(target2, 'foo')) 
```

返回如下结果：![](https://img-blog.csdnimg.cn/20210319155256751.png#pic_center)


Object.getOwnPropertyDescriptors()的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/239adbf5ebbcec07d551cc8d41f3c673.png)

ES9新特性
------

### 1.for await of

for of方法能够遍历具有Symbol.iterator接口的同步迭代器数据，但是不能遍历异步迭代器。
ES9新增的for await of可以用来遍历具有Symbol.asyncIterator方法的数据结构，也就是异步迭代器，且会等待前一个成员的状态改变后才会遍历到下一个成员，相当于async函数内部的await。现在我们有三个异步任务，想要实现依次输出结果，该如何实现呢？

```javascript
// for of遍历
function Gen (time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve(time)
    }, time)
  })
}
async function test () {
  let arr = [Gen(2000), Gen(100), Gen(3000)]
  for (let item of arr) {
    console.log(Date.now(), item.then(console.log))
  }
}
test() 
```

得到如下结果：
![](https://img-blog.csdnimg.cn/img_convert/e4f1f0a82266af7e089b8aec019ef452.png)

上述代码证实了for of方法不能遍历异步迭代器，得到的结果并不是我们所期待的，于是for await of就粉墨登场啦！

```javascript
function Gen (time) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve(time)
    }, time)
  })
}
async function test () {
  let arr = [Gen(2000), Gen(100), Gen(3000)]
  for await (let item of arr) {
    console.log(Date.now(), item)
  }
}
test()
// 1575536194608 2000
// 1575536194608 100
// 1575536195608 3000 
```

使用for await of遍历时，会等待前一个Promise对象的状态改变后，再遍历到下一个成员。

异步迭代器的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/70109e5ad723a074492bfbae90d35f17.png)

### 2.Object Rest Spread

ES6中添加的最意思的特性之一是spread操作符。你不仅可以用它替换cancat()和slice()方法，使数组的操作(复制、合并)更加简单，还可以在数组必须以拆解的方式作为函数参数的情况下，spread操作符也很实用。

```javascript
const arr1 = [10, 20, 30];
const copy = [...arr1]; // 复制
console.log(copy);    // [10, 20, 30]
const arr2 = [40, 50];
const merge = [...arr1, ...arr2]; // 合并
console.log(merge);    // [10, 20, 30, 40, 50]
console.log(Math.max(...arr));    // 30 拆解 
```

ES9通过向对象文本添加扩展属性进一步扩展了这种语法。他可以将一个对象的属性拷贝到另一个对象上，参考以下情形:

```javascript
const input = {
  a: 1,
  b: 2,
  c: 1
}
const output = {
  ...input,
  c: 3
}
console.log(output) // {a: 1, b: 2, c: 3} 
```

上面代码可以把 input 对象的数据都添加到 output 对象中，需要注意的是，**如果存在相同的属性名，只有最后一个会生效**。

```javascript
const input = {
  a: 1,
  b: 2
}
const output = {
  ...input,
  c: 3
}
input.a='浪里行舟'
console.log(input,output) // {a: "浪里行舟", b: 2} {a: 1, b: 2, c: 3} 
```

上面例子中，修改input对象中的值，output并没有改变，说明扩展运算符拷贝一个对象（类似这样obj2 = {...obj1}），**实现只是一个对象的浅拷贝**。值得注意的是，如果属性的值是一个对象的话，该对象的引用会被拷贝：

```javascript
const obj = {x: {y: 10}};
const copy1 = {...obj};    
const copy2 = {...obj}; 
obj.x.y='浪里行舟'
console.log(copy1,copy2) // x: {y: "浪里行舟"} x: {y: "浪里行舟"}
console.log(copy1.x === copy2.x);    // → true 
```

copy1.x 和 copy2.x 指向同一个对象的引用，所以他们严格相等。

我们再来看下 Object rest 的示例：

```javascript
const input = {
  a: 1,
  b: 2,
  c: 3
}
let { a, ...rest } = input
console.log(a, rest) // 1 {b: 2, c: 3} 
```

当对象 key-value 不确定的时候，把必选的 key 赋值给变量，用一个变量收敛其他可选的 key 数据，这在之前是做不到的。注意，**rest属性必须始终出现在对象的末尾**，否则将抛出错误。

Rest与Spread兼容性一致，下列以spread为例：![](https://img-blog.csdnimg.cn/20210319155147489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1haU0pfRnJvbnQ=,size_16,color_FFFFFF,t_70#pic_center)

### 3.Promise.prototype.finally()

Promise.prototype.finally() 方法返回一个Promise，在promise执行结束时，无论结果是fulfilled或者是rejected，在执行then()和catch()后，都会执行finally指定的回调函数。

```javascript
fetch('https://www.google.com')
  .then((response) => {
    console.log(response.status);
  })
  .catch((error) => { 
    console.log(error);
  })
  .finally(() => { 
    document.querySelector('#spinner').style.display = 'none';
  }); 
```

无论操作是否成功，当您需要在操作完成后进行一些清理时，finally()方法就派上用场了。这为指定执行完promise后，无论结果是fulfilled还是rejected都需要执行的代码提供了一种方式，**避免同样的语句需要在then()和catch()中各写一次的情况**。

Promise.prototype.finally()的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/c1c488e428ea44481e8c66086bfff3ca.png)

### 4.新的正则表达式特性

ES9为正则表达式添加了四个新特性，进一步提高了JavaScript的字符串处理能力。这些特点如下:

*   s (dotAll) 标志
*   命名捕获组
*   Lookbehind 后行断言
*   Unicode属性转义

#### （1）s(dotAll)flag

正则表达式中，点（.）是一个特殊字符，代表任意的单个字符，但是有两个例外。一个是四个字节的 UTF-16 字符，这个可以用u修饰符解决；另一个是行终止符,如换行符(n)或回车符(r),这个可以通过ES9的s(dotAll)flag，在原正则表达式基础上添加s表示:

```javascript
console.log(/foo.bar/.test('foonbar')) // false
console.log(/foo.bar/s.test('foonbar')) // true 
```

那如何判断当前正则是否使用了 dotAll 模式呢？

```javascript
const re = /foo.bar/s // Or, `const re = new RegExp('foo.bar', 's');`.
console.log(re.test('foonbar')) // true
console.log(re.dotAll) // true
console.log(re.flags) // 's' 
```

#### （2）命名捕获组

在一些正则表达式模式中，使用数字进行匹配可能会令人混淆。例如，使用正则表达式/(d{4})-(d{2})-(d{2})/来匹配日期。因为美式英语中的日期表示法和英式英语中的日期表示法不同，所以很难区分哪一组表示日期，哪一组表示月份:

```javascript
const re = /(d{4})-(d{2})-(d{2})/;
const match= re.exec('2019-01-01');
console.log(match[0]);    // → 2019-01-01
console.log(match[1]);    // → 2019
console.log(match[2]);    // → 01
console.log(match[3]);    // → 01 
```

ES9引入了命名捕获组，允许为每一个组匹配指定一个名字，既便于阅读代码，又便于引用。

```javascript
const re = /(?<year>d{4})-(?<month>d{2})-(?<day>d{2})/;
const match = re.exec('2019-01-01');
console.log(match.groups);          // → {year: "2019", month: "01", day: "01"}
console.log(match.groups.year);     // → 2019
console.log(match.groups.month);    // → 01
console.log(match.groups.day);      // → 01 
```

上面代码中，“命名捕获组”在圆括号内部，模式的头部添加“问号 + 尖括号 + 组名”（?），然后就可以在exec方法返回结果的groups属性上引用该组名。

命名捕获组也可以使用在replace()方法中，例如将日期转换为美国的 MM-DD-YYYY 格式：

```javascript
const re = /(?<year>d{4})-(?<month>d{2})-(?<day>d{2})/
const usDate = '2018-04-30'.replace(re, '$<month>-$<day>-$<year>')
console.log(usDate) // 04-30-2018 
```

#### （3）Lookbehind 后行断言

JavaScript 语言的正则表达式，只支持先行断言，不支持后行断言，先行断言我们可以简单理解为"先遇到一个条件，再判断后面是否满足"，如下面例子：

```javascript
let test = 'hello world'
console.log(test.match(/hello(?=sworld)/))
// ["hello", index: 0, input: "hello world", groups: undefined] 
```

但有时我们想判断前面是 world 的 hello，这个代码是实现不了的。在 ES9 就支持这个后行断言了：

```javascript
let test = 'world hello'
console.log(test.match(/(?<=worlds)hello/))
// ["hello", index: 6, input: "world hello", groups: undefined] 
```

**(?<…)是后行断言的符号，(?..)是先行断言的符号**，然后结合 =(等于)、!(不等)、1(捕获匹配)。

#### （4）Unicode属性转义

ES2018 引入了一种新的类的写法p{...}和P{...}，允许正则表达式匹配符合 Unicode 某种属性的所有字符。比如你可以使用p{Number}来匹配所有的Unicode数字，例如，假设你想匹配的Unicode字符㉛字符串:

```javascript
const str = '㉛';
console.log(/d/u.test(str));    // → false
console.log(/p{Number}/u.test(str));     // → true 
```

同样的，你可以使用p{Alphabetic}来匹配所有的Unicode单词字符:

```javascript
const str = 'ض';
console.log(/p{Alphabetic}/u.test(str));     // → true
// the w shorthand cannot match ض
console.log(/w/u.test(str));    // → false 
```

同样有一个负向的Unicode属性转义模板 P{...}

```javascript
console.log(/P{Number}/u.test('㉛'));    // → false
console.log(/P{Number}/u.test('ض'));    // → true
console.log(/P{Alphabetic}/u.test('㉛'));    // → true
console.log(/P{Alphabetic}/u.test('ض'));    // → false 
```

除了字母和数字之外，Unicode属性转义中还可以使用其他一些属性。

以上这几个特性的支持情况
![](https://img-blog.csdnimg.cn/img_convert/c109770e67544b955444062d047dde5d.png)

ES10新特性
-------

### 1.Array.prototype.flat()

多维数组是一种常见的数据格式，特别是在进行数据检索的时候。将多维数组打平是个常见的需求。通常我们能够实现，但是不够优雅。

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```javascript
newArray = arr.flat(depth) // depth是指定要提取嵌套数组的结构深度，默认值为 1 
```

接下来我们看两个例子：

```javascript
const numbers1 = [1, 2, [3, 4, [5, 6]]]
console.log(numbers1.flat())// [1, 2, 3, 4, [5, 6]]
const numbers2 = [1, 2, [3, 4, [5, 6]]]
console.log(numbers2.flat(2))// [1, 2, 3, 4, 5, 6] 
```

上面两个例子说明flat 的参数没有设置，取默认值 1，也就是说只扁平化第一级；当 flat 的参数大于等于 2，返回值就是 [1, 2, 3, 4, 5, 6] 了。

Array.prototype.flat的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/732d74b68d46a42de5ee1672e01f7b86.png)

### 2.Array.prototype.flatMap()

有了flat方法，那自然而然就有Array.prototype.flatMap方法，flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。从方法的名字上也可以看出来它包含两部分功能一个是 map，一个是 flat（深度为1）。

```javascript
let arr = [1, 2, 3]
console.log(arr.map(item => [item * 2]).flat()) // [2, 4, 6]
console.log(arr.flatMap(item => [item * 2])) // [2, 4, 6] 
```

实际上flatMap是综合了map和flat的操作，所以**它也只能打平一层**。

Array.prototype.flatmap的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/ea937f5e00140200b7fc67ef722dc5e0.png)

### 3.Object.fromEntries()

Object.fromEntries 这个新的API实现了与 Object.entries 相反的操作。这使得根据对象的 entries 很容易得到 object。

```javascript
const object = { x: 23, y:24 };
const entries = Object.entries(object); // [['x', 23], ['y', 24]]
const result = Object.fromEntries(entries); // { x: 23, y: 24 } 
```

ES2017引入了Object.entries, 这个方法可以将对象转换为数组,这样对象就可以使用数组原型中的众多内置方法，比如map, filter、reduce，举个例子，我们想提取下列对象obj中所有value大于21的键值对，如何操作呢？

```javascript
// ES10之前
const obj = {
  a: 21,
  b: 22,
  c: 23
}
console.log(Object.entries(obj)) // [['a',21],["b", 22],["c", 23]]
let arr = Object.entries(obj).filter(([a, b]) => b > 21) // [["b", 22],["c", 23]]
let obj1 = {}
for (let [name, age] of arr) {
  obj1[name] = age
}
console.log(obj1) // {b: 22, c: 23} 
```

上例中得到了数组arr，想再次转化为对象，就需要手动写一些代码来处理，但是有了Object.fromEntries()就很容易实现

```javascript
// 用Object.fromEntries()来实现
const obj = {
  a: 21,
  b: 22,
  c: 23
}
let res = Object.fromEntries(Object.entries(obj).filter(([a, b]) => b > 21))
console.log(111, res) // {b: 22, c: 23} 
```

Object.fromEntries()的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/6e0bed7e6e79eb99ad49442d38553b92.png)

### 4.String.trimStart 和 String.trimEnd

移除开头和结尾的空格，之前我们用正则表达式来实现，现在ES10新增了两个新特性，让这变得更简单！

trimStart() 方法从字符串的开头删除空格，trimLeft()是此方法的别名。

```javascript
let str = ' 前端工匠 '
console.log(str.length) // 6
str = str.trimStart()
console.log(str.length) // 5
let str1 = str.trim() // 清除前后的空格
console.log(str1.length) // 4
str.replace(/^s+/g, '') // 也可以用正则实现开头删除空格 
```

trimEnd() 方法从一个字符串的右端移除空白字符，trimRight 是 trimEnd 的别名。

```javascript
let str = ' 浪里行舟 '
console.log(str.length) // 6
str = str.trimEnd()
console.log(str.length) // 5
let str1 = str.trim() //清除前后的空格
console.log(str1.length) // 4
str.replace(/s+$/g, '') // 也可以用正则实现右端移除空白字符 
```

String.trimStart和String.trimEnd 两者兼容性一致，下图以trimStart为例：
![](https://img-blog.csdnimg.cn/img_convert/d2766fe9167f9729b104735e942e6455.png)

### 5.String.prototype.matchAll

如果一个正则表达式在字符串里面有多个匹配，现在一般使用g修饰符或y修饰符，在循环里面逐一取出。

```javascript
function collectGroup1 (regExp, str) {
  const matches = []
  while (true) {
    const match = regExp.exec(str)
    if (match === null) break
    matches.push(match[1])
  }
  return matches
}
console.log(collectGroup1(/"([^"]*)"/g, `"foo" and "bar" and "baz"`))
// [ 'foo', 'bar', 'baz' ] 
```

值得注意的是，如果没有修饰符 /g, .exec() 只返回第一个匹配。现在通过ES9的String.prototype.matchAll方法，可以一次性取出所有匹配。

```javascript
function collectGroup1 (regExp, str) {
  let results = []
  for (const match of str.matchAll(regExp)) {
    results.push(match[1])
  }
  return results
}
console.log(collectGroup1(/"([^"]*)"/g, `"foo" and "bar" and "baz"`))
// ["foo", "bar", "baz"] 
```

上面代码中，由于string.matchAll(regex)返回的是遍历器，所以可以用for...of循环取出。

String.prototype.matchAll的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/533379f54aca27b2033b242e52cfeab6.png)

### 6.try…catch

在ES10中，try-catch语句中的参数变为了一个可选项。以前我们写catch语句时，必须传递一个异常参数。这就意味着，即便我们在catch里面根本不需要用到这个异常参数也必须将其传递进去

```javascript
// ES10之前
try {
  // tryCode
} catch (err) {
  // catchCode
} 
```

这里 err 是必须的参数，在 ES10 可以省略这个参数：

```javascript
// ES10
try {
  console.log('Foobar')
} catch {
  console.error('Bar')
} 
```

try…catch的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/06ee4178c87378722015bf78dfbd8cd5.png)

### 7.BigInt

JavaScript 所有数字都保存成 64 位浮点数，这给数值的表示带来了两大限制。一是数值的精度只能到 53 个二进制位（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示的，这使得 JavaScript 不适合进行科学和金融方面的精确计算。二是大于或等于2的1024次方的数值，JavaScript 无法表示，会返回Infinity。

```javascript
// 超过 53 个二进制位的数值，无法保持精度
Math.pow(2, 53) === Math.pow(2, 53) + 1 // true
// 超过 2 的 1024 次方的数值，无法表示
Math.pow(2, 1024) // Infinity 
```

现在ES10引入了一种新的数据类型 BigInt（大整数），来解决这个问题。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

创建 BigInt 类型的值也非常简单，只需要在数字后面加上 n 即可。例如，123 变为 123n。也可以使用全局方法 BigInt(value) 转化，入参 value 为数字或数字字符串。

```javascript
const aNumber = 111;
const aBigInt = BigInt(aNumber);
aBigInt === 111n // true
typeof aBigInt === 'bigint' // true
typeof 111 // "number"
typeof 111n // "bigint" 
```

如果算上 BigInt，JavaScript 中原始类型就从 6 个变为了 7 个。

*   Boolean
*   Null
*   Undefined
*   Number
*   String
*   Symbol (new in ECMAScript 2015)
*   BigInt (new in ECMAScript 2019)

BigInt的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/adc753d85145fc4ad6665b39ce7ff4b3.png)

### 8.Symbol.prototype.description

我们知道，Symbol 的描述只被存储在内部的 [[Description]]，没有直接对外暴露，我们只有调用 Symbol 的 toString() 时才可以读取这个属性：

```javascript
Symbol('desc').description;  // "desc"
Symbol('').description;      // ""
Symbol().description;        // undefined 
```

Symbol.prototype.description的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/279ed8f6ad607ccb00d58f0ee47e2903.png)

### 9.Function.prototype.toString()

ES2019中，Function.toString()发生了变化。之前执行这个方法时，得到的字符串是去空白符号的。而现在，得到的字符串呈现出原本源码的样子：

```javascript
function sum(a, b) {
  return a + b;
}
console.log(sum.toString());
// function sum(a, b) {
//  return a + b;
// } 
```

Function.prototype.toString()的支持情况：
![](https://img-blog.csdnimg.cn/img_convert/ee3d27b9ce81c7ba21974b92689ad5d5.png)

## 后记

如需前端指导、前端资料、Java指导和 Java 资料的请联系本人，感谢您的支持。

> WECHAT：xzsj07
> 备注：加好友请注明来源。


