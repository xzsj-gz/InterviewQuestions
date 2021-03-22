前言
---

一个ECMAScript标准的制作过程，包含了Stage 0到Stage 4 五个阶段，每个阶段提交至下一阶段都需要TC39审批通过。本文介绍这些新特性处于Stage 3 或者Stage 4 阶段，这意味着应该很快在浏览器和其他引擎中支持这些特性。

一、类的私有变量
---

最新提案之一是在类中添加私有变量的方法。我们将**使用 # 符号表示类的私有变量**。这样就不需要使用闭包来隐藏不想暴露给外界的私有变量。

```javascript
class Counter {
  #x = 0;

  #increment() {
    this.#x++;
  }

  onClick() {
    this.#increment();
  }
}

const c = new Counter();
c.onClick(); // 正常
c.#increment(); // 报错 
```

通过 # 修饰的成员变量或成员函数就成为了私有变量，如果试图在 Class 外部访问，则会抛出异常。现在，此特性可在最新版本的 Chrome 和 Node.js中使用。

二、可选链操作符
---

你可能碰到过这样的情形：当需要访问嵌套在对象内部好几层的属性时，会得到臭名昭著的错误`Cannot read property 'stop' of undefined`,然后你就要修改你的代码来处理来处理属性链中每一个可能的undefined对象，比如：

```javascript
let nestedProp = obj && obj.first && obj.first.second; 
```

在访问obj.first.second之前，obj和obj.first 的值要被确认非null(且不是undefined)。目的是为了防止错误发生，如果简单直接的访问obj.first.second而不对obj和obj.first 进行校验就有可能产生错误。

有了可选链式调用 ，你只要这样写就可以做同样的事情:

```javascript
let nestedProp = obj?.first?.second; 
```

如果obj或obj.first是null/undefined，表达式将会短路计算直接返回undefined。

三、空位合并操作符
---

我们在开发过程中，经常会遇到这样场景：变量如果是空值，则就使用默认值，我们是这样实现的：

```javascript
let c = a ? a : b // 方式1
let c = a || b // 方式2 
```

这两种方式有个明显的弊端，它都会覆盖所有的假值，如(0, '', false)，这些值可能是在某些情况下有效的输入。

为了解决这个问题，有人提议创建一个“nullish”合并运算符，用 ?? 表示。**有了它，我们仅在第一项为 null 或 undefined 时设置默认值**。

```javascript
let c = a ?? b;
// 等价于let c = a !== undefined && a !== null ? a : b; 
```

例如有以下代码：

```javascript
const x = null;
const y = x ?? 500;
console.log(y); // 500
const n = 0
const m = n ?? 9000;
console.log(m) // 0 
```

四、BigInt
---

JS在Math上一直很糟糕的原因之一是，**无法精确表示大于的数字2 ^ 53**，这使得处理相当大的数字变得非常困难。

```javascript
1234567890123456789 * 123;
// -> 151851850485185200000 // 计算结果丢失精度 
```

幸运的是，BigInt（大整数）就是来解决这个问题。你可以在BigInt上使用与普通数字相同的运算符，例如 +, -, /, *, %等等。

创建 BigInt 类型的值也非常简单，只需要在数字后面加上 n 即可。例如，123 变为 123n。也可以使用全局方法 BigInt(value) 转化，入参 value 为数字或数字字符串。

```javascript
const aNumber = 111;
const aBigInt = BigInt(aNumber);
aBigInt === 111n // true
typeof aBigInt === 'bigint' // true
typeof 111 // "number"
typeof 111n // "bigint" 
```

只要在数字末尾加上 n，就可以正确计算大数了：

```javascript
1234567890123456789n * 123n;
// -> 151851850485185185047n 
```

不过有一个问题，在大多数操作中，不能将 BigInt与Number混合使用。比较Number和 BigInt是可以的，但是不能把它们相加。

```javascript
1n < 2 
// true

1n + 2
// Uncaught TypeError: Cannot mix BigInt and other types, use explicit conversions 
```

现在，此特性可在最新版本的 Chrome 和 Node.js中使用。

五、static 字段
---

它允许类拥有静态字段，类似于大多数OOP语言。静态字段可以用来代替枚举，也可以用于私有字段。

```javascript
class Colors {
  // public static 字段
  static red = '#ff0000';
  static green = '#00ff00';

  // private static 字段
  static #secretColor = '#f0f0f0';

}

font.color = Colors.red;
font.color = Colors.#secretColor; // 出错 
```

现在，此特性可在最新版本的 Chrome 和 Node.js中使用。

六、Top-level await
---

ES2017（ES8）中的 async/await 特性仅仅允许在 async 函数内使用 await 关键字，新的提案旨在允许 await 关键字在顶层内容中的使用，例如可以简化动态模块加载的过程：

```javascript
const strings = await import(`/i18n/${navigator.language}`); 
```

这个特性在浏览器控制台中调试异步内容（如 fetch）非常有用，而无需将其包装到异步函数中。
![](https://img-blog.csdnimg.cn/img_convert/c008f68d5aeb8b54d72ac351cd938f19.png)

另一个使用场景是，可以在以异步方式初始化的 ES 模块的顶层使用它(比如建立数据库连接)。当导入这样的“异步模块”时，模块系统将等待它被解析，然后再执行依赖它的模块。这种处理异步初始化方式比当前返回一个初始化promise并等待它解决来得更容易。一个模块不知道它的依赖是否异步。

```javascript
// db.mjs
export const connection = await createConnection(); 
```

```javascript
// server.mjs
import { connection } from './db.mjs';
server.start(); 
```

在此示例中，在server.mjs中完成连接之前不会执行任何操作db.mjs。

现在，此特性可在最新版本的 Chrome中使用。

七、WeakRef
---

一般来说，在 JavaScript 中，对象的引用是强保留的，这意味着只要持有对象的引用，它就不会被垃圾回收。

```javascript
const ref = { x: 42, y: 51 };
// 只要我们访问 ref 对象（或者任何其他引用指向该对象），这个对象就不会被垃圾回收 
```

目前在 Javascript 中，WeakMap 和 WeakSet 是弱引用对象的唯一方法：将对象作为键添加到 WeakMap 或 WeakSet 中，是不会阻止它被垃圾回收的。

```javascript
const wm = new WeakMap();
{
  const ref = {};
  const metaData = 'foo';
  wm.set(ref, metaData);
  wm.get(ref);
  // 返回 metaData
}
// 在这个块范围内，我们已经没有对 ref 对象的引用。
// 因此，虽然它是 wm 中的键，我们仍然可以访问，但是它能够被垃圾回收。

const ws = new WeakSet();
ws.add(ref);
ws.has(ref);// 返回 true 
```

**JavaScript 的 WeakMap 并不是真正意义上的弱引用**：实际上，只要键仍然存活，它就强引用其内容。WeakMap 仅在键被垃圾回收之后，才弱引用它的内容。

WeakRef 是一个更高级的 API，它提供了真正的弱引用，Weakref 实例具有一个方法 deref，该方法返回被引用的原始对象，如果原始对象已被收集，则返回undefined对象。

```javascript
const cache = new Map();
const setValue =  (key, obj) => {
  cache.set(key, new WeakRef(obj));
};

const getValue = (key) => {
  const ref = cache.get(key);
  if (ref) {
    return ref.deref();
  }
};

// this will look for the value in the cache
// and recalculate if it's missing
const fibonacciCached = (number) => {
  const cached = getValue(number);
  if (cached) return cached;
  const sum = calculateFibonacci(number);
  setValue(number, sum);
  return sum;
}; 
```

总而言之，JavaScript 中对象的引用是强引用，WeakMap 和 WeakSet 可以提供部分的弱引用功能，若想在 JavaScript 中实现真正的弱引用，可以通过配合使用 WeakRef 和终结器（Finalizer）来实现。

现在，此特性可在最新版本的 Chrome 和 Node.js中使用。

## 后记

如需前端指导、前端资料、Java指导和 Java 资料的请联系本人，感谢您的支持。

> WECHAT：xzsj07
> 备注：加好友请注明来源。
