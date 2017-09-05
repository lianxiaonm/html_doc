
> 面试常规知识点

## JavaScript
* 什么是`闭包`、`作用域`、`原型`和`原型链`：
    ```
    闭包：在非定义作用域出执行的方法都会产生闭包 -- 涉及到词法作用域和静态作用域
    作用域：一个function既是一个作用域
    原型和原型链：
        1. 所有的对象都有__proto__属性，该属性对应该对象的原型.
        2. 所有的函数对象都有prototype属性，该属性的值会被赋值给该函数创建的对象的_proto_属性.
        3. 所有的原型对象都有constructor属性，该属性对应创建所有指向该原型的实例的构造函数.
        4. 函数对象和原型对象通过prototype和constructor属性进行相互关联.
    -- 拓展:（个人认知）
        作用域在某个层面上和原型差不多。作用域内部对象的查找和对象上面的属性查找很类似，从当前对象（作用域）查找，
    一直往上，到最顶层原型(作用域)，作用域内部的属性就好比对象某一层原型上的属性一般。
    ```
* `this` 指针
   ```
   误区：
       1、指向函数自身
       2、指向函数作用域
   四种绑定方式：
       1、默认绑定 -- 严格模式下为undefined，非严格模式下完全取决于调用位置
       2、call apply 显式绑定
       3、隐式绑定 -- 带有修饰符方式的函数调用，有时候会存在一定的隐式丢失
       4、new 绑定 -- 1、创建对象 2、构造原型并连接 3、新对象绑定到this 4、无返回则返回新对象
   ```
* 数组去重
   ```
   ES6的解决方案：[...new Set(Arr)] -- 先转成set在拷贝数组
   普通方案：新建数组，对原数组进行循环遍历
   ```
* 遍历自己的属性，不包含原型和不可枚举属性
    ```
    1、Object.keys       //返回对象自身的属性，不包括原型和不可枚举的属性
    2、hasOwnProperty    //判断key是否为自身属性
    3、for...of          //ES6的语法糖
    ```
* cookie,sessionStorage和localStorage的区别
    ```
    共同点：域名绑定，存储数据
    区别：
        1、cookie伴随请求，跨浏览器。存储量小,有时效性
        2、storage不能跨浏览器。session是会话层。local是存在本地。存储量较大
    ```
* CORS跨域问题 [博客](http://newhtml.net/using-cors/)
    ```
    默认的CORS请求不携带cookies
    response:
        Access-Control-Allow-Origin（必选）：* 或者域名数组
        Access-Control-Allow-Credentials（可选）：true -- 配合前端withCredentials
        Access-Control-Allow-Headers（可选）-- 允许的请求头
    request:
        withCredentials ：true -- 让请求携带cookie
    ```
* requestAnimationFrame
    ```
    requestAnimationFrame:在浏览器重绘之前调用回调方法。 --  一般做逐帧动画使用
        优点:降低重绘频率
    ```
* js点透
    ```
    场景：A/B有重叠部分点击了就会出现点透现象1、A/B两层的z轴重叠 2、上层A点击后消失 3、B层有click事件
    原因：click的延迟
    方案：
        1、统一使用touch事件。避免用click
        2、A层阻止默认点击事件 -- e.preventDefault
        3、浮层的点击增加小延迟。
    ```


## CSS



## 正则表达式
* `?` 的含义
    ```
    1、非获取匹配
    2、0或1次。等价于{0,1}
    3、紧跟任意字符表示【非贪婪】的匹配模式
    ```
* 字符串按位格式化
    ```
    1、'1234565432'.match(/(\d{4}|\d+)/g).join(' ')
    2、'12345643212345'.replace(/(\d{4})/g,'$1 ')
    ```
* 其他
    ```
    ^ -- 开头
    $ -- 结尾
    $1 $2 ..  匹配原字符
    ```

## Web安全问题
* XSS漏洞 [Cross-Site Scripting](https://en.wikipedia.org/wiki/Cross-site_scripting)
    ```
    解决办法:
        1、在不同上下文中，使用合适的 escape 方式
        2、不要相信 任何 来自用户的输入（不仅限于 POST Body，还包括 QueryString，甚至是 Headers）
    ```
* CSRF漏洞 [Cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
    ```
    解决办法:
        1、给所有请求加上 token 检查。token 一般是随机字符串，只需确保其不可预测性即可
        2、检查 referer -- 无法防御自身的攻击
    ```

## Webpack
* webpack的



## Framework（Vue2.x Angular1.x React...）
* 三大框架的共同特点：
    ```
    Vue2.x的mixins      -- 混合功能
    React的extend        -- 继承
    Angular1.x的decorator   -- 装饰器
    ```
* Vue2.x
* React
* Angular1.x


