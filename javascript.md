> 这是JavaScript知识点的一个集合。

#### JavaScript 浮点数陷阱及解法
-----------
> [参考](https://zhuanlan.zhihu.com/p/30703042)

#### Microtasks Macrotasks
-----------
> [参考](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

* microtasks:
    ```
    process.nextTick
    promise
    Object.observe
    ```
* macrotasks:
    ```
    setTimeout
    setInterval
    setImmediate
    I/O
    ```

#### 关于babel的 stage-X 概念
-----------
```
Stage 0 - Strawman（展示阶段）
Stage 1 - Proposal（征求意见阶段）
Stage 2 - Draft（草案阶段）
Stage 3 - Candidate（候选人阶段）
Stage 4 - Finished（定案阶段）
```


#### ES6
-----------
> [ECMAScript 6 入门](http://es6.ruanyifeng.com/)

* `let`中注意点
    1. TDZ(暂时性死区) -- `使用let命令声明变量之前，该变量都是不可用的` -- babel之后不会存在这样的问题
        ```javascript
        // typeof 不在是一个100%安全的操作
        typeof x;   // ReferenceError
        typeof b;   // undefined
        let x;
        ```
    2. 不存在变量提升
        ```javascript
        // var 的情况
        console.log(foo); // 输出undefined
        var foo = 2;

        // let 的情况
        console.log(bar); // 报错ReferenceError
        let bar = 2;
        ```
    3. 不允许重复声明
* `do` 表达式
    ```javascript
    // ES6
    let x = do {
      let t = f();
      t * t + 1;
    };
    //ES5
    var a = function () {
      var t = f();
      return t * t + 1;
    }();
    ```
* `const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个`内存地址`不得改动
* 六种声明变量的方法
    1. 兼容：var function -- 全局变量 ＝ 顶层对象
    2. let const class import -- 全局变量 ≠ 顶层对象（window or global） -- 在ES6中
* 变量的解构和赋值
    1. 解构： -- 解构变量可以赋予默认值
        ```javascript
        let [x,y,z] = [1,2];
        let { a, a: { b }, a: { b: { c, d = 5 } } } = { a: { b: { c: 1 } } }

        x //1
        y //2
        z // undeinfed
        a // { b: { c: 1 } }
        b // { c: 1 }
        c // 1
        d // 5
        ```
    2. 赋值
        ```javascript
        let obj = {};
        let arr = [];

        ({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

        obj // {prop:123}
        arr // [true]
        ```
    3. 对于一个已声明的变量进行解构赋值，需要注意 -- `因为 JavaScript 引擎会将{x}理解成一个代码块`
        ```javascript
        // 错误的写法
        let x;
        {x} = {x: 1};
        // SyntaxError: syntax error
        // 正确的写法
        ({x} = {x: 1});
        ```
    4. 特殊解构
        ```javascript
        // 数组使用对象属性解构
        let arr = [1, 2, 3];
        let {0 : first, [arr.length - 1] : last} = arr;
        first // 1
        last // 3
        // 字符串解构
        const [a, b, c, d, e] = 'hello';
        ```
* String的拓展方法
    1. `codePointAt` --  正确处理4个字节的字符（常规utf-16的每个字符是2个字节。对于4个字节的字符js会认为他们是2个字符。如：`𠮷`） 返回字符的utf-16的编码
        ```javascript
        var s = "𠮷";    // 0xD842 0xDFB7 -- 55362 57271
        s.length // 2
        s.charAt(0) // ''
        s.charCodeAt(0) // 55362
        ```
    2. `fromCodePoint` -- 正好与`codePointAt`方法相反。
    3. `at` -- 对`charAt`的补充，识别编码大于0xFFFF的字符，需要[polyfill库](https://github.com/es-shims/String.prototype.at)
    4. `includes`、`startsWith`、`endsWith`
    5. `repeat` 对字符的重复n次，n取整且不能小于-1和Infinity。。NaN等同于0
    6. `padStart`头部补全、`padEnd`尾部补全 是ES2017字符串补全的功能。支持2个入参。
        ```javascript
        'x'.padStart(5, 'ab')   // 'ababx'
        'x'.padEnd(5, 'ab')     // 'xabab'
        'abc'.padStart(10, '0123456789')    // '0123456abc'
        'x'.padStart(4)         // '   x'
        ```
* RegExp的拓展
    1. 后行断言
    2. 解构赋值和替换
* Number的拓展 [Number](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)
    1. `isSafeInteger()` 判断一个整数是否在精准范围内 `MAX_SAFE_INTEGER(2^53)` ~ `MIN_SAFE_INTEGER(-2^53)`
    2. `EPSILON` 极小的常量 -- 用于浮点计算的允许误差范围
    3. Math的拓展方法：
        1. `trunc`用于去除一个数的小数部分
        2. `sign`判断一个数字是正数、负数、还是0
        3. `cbrt`计算一个数的立方根
        4. `clz32`返回一个数的32位二进制有多少个前导0
        5. `imul` 大多数情况下，`imul(a, b)`与`a * b`的结果是相同的，即该方法等同于`(a * b)|0`的效果。对于那些很大的数的乘法，低位数值往往都是不精确的，`imul`方法可以返回正确的低位数值。
        6. `fround`、`hypot`、
        7. 对数方法：`expm1`、`log1p`、`log10`、`log2`
        8. 双曲函数: `sinh`双曲正弦、`cosh`双曲余弦、`tanh`双曲正切、`asinh`反双曲正弦、`acosh`反双曲余弦、`atanh`反双曲正切
    4. 指数运算符,`**`  | 在 V8 引擎中，指数运算符与Math.pow的实现不相同，对于特别大的运算结果，两者会有细微的差异。
        ```javascript
        2 ** 3          // 8
        Math.pow(2,3)   // 8
        ```
* 函数的拓展
    1. 箭头函数 -- 注意点：
        1. 无内部的`this`。所以当然也就不能用`call()`、`apply()`、`bind()`这些方法去改变this的指向
        2. 不可以`new`
        3. 不可以使用`argument`，如果需要使用。请使用rest参数`(...arg)`
        4. 不可以使用`yield`，因此箭头函数不能用作Generator函数
    2. 双冒号运算符 `::` -- `call`、`apply`、`bind`的简化
    3. `尾调用`和`尾递归`的性能优化 -- （只保留内层函数的调用帧）
* 数组的拓展 [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
    1. 拓展运算符（`...`），用户解构，合并，复制，toArray
    2. `from`(将类数组和可遍历的对象转成数组)、`of`(将一组值转成数组)
    3. `copyWithin`将数组内部从某个位置开始拷贝到当前数组的某个位置
    4. `find`和`findIndex`,找寻数组中复合条件的第一个成员。一个返回内容。一个返回下标
    5. `fill`用指定的值填充数组
    6. `entries`、`keys`、`values`用于遍历数组
    7. `includes` 和字符串的includes类似
* 对象的拓展
    1. 属性表达式
        ```javascript
        let propKey = 'foo';
        let obj = {
          [propKey]: true,
          ['a' + 'bc']: 123
        };
        ```
    2. `assign` -- 浅拷贝,复制函数则是将函数求值之后再复制，数组则视为对象处理
    3. `getOwnPropertyDescriptor`获取属性的描述
        ```
        Object.getOwnPropertyDescriptor({ foo: 123 }, 'foo')
        //{
        //    value: 123,         // 值
        //    writable: true,     // 是否可写入
        //    enumerable: true,   // 可枚举性
        //    configurable: true  // 可配置
        //}
        ```
    3. 遍历
        1. `for...in` 遍历自身和继承的可枚举属性
        2. `Object.keys()` 返回自身可枚举属性的
        3. `Object.getOwnPropertyNames(obj)` 返回自身的所有属性，包括不可枚举的，不包括Symbol属性
        4. `Object.getOwnPropertySymbols(obj)` 返回自身的所有的所有所有属性，包括Symbol属性
        5. `Reflect.ownKeys(obj)` 返回自身的所有属性，all
    4. `getOwnPropertyDescriptors` ES2017引入 。。 返回对象所有自身属性的描述对象
    5. `super`关键字
    6. Null 传导运算符 -- `提案`
        ```
        // 老的写法
        const firstName = (message
          && message.body
          && message.body.user
          && message.body.user.firstName) || 'default';
        // 新写法
        const firstName = message?.body?.user?.firstName || 'default';
        ```
* `Symbol` 类型 -- `待定`
* `Promise` 对象
* `Generator` 函数

