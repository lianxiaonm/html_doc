> 面试常规知识点

## JavaScript
* js创建的对象的方式
    ```
    1、 var obj = {} // new Object() .. 字面量
    2、 var M = function(){};    //  .. 构造函数
        var obj = new M();
    3、 var p = Object.create(); //  .. Object.create方法
    ```

* js new Foo()发生了什么
    ```
    1.创建了一个新对象
    2.将this指向这个新对象
    3.执行构造函数里面的代码
    4.返回新对象（this）
    ```

* js 实现继承的方式
    1. 借用构造函数实现继承
        ```
        function Parent1(){
            this.name = "parent1"
        }
        function Child1(){
            Parent1.call(this);
            this.type = "child1";
        }
        //缺点：Child1无法继承Parent1的原型对象，并没有真正的实现继承（部分继承）
        ```
    2. 借用原型链实现继承
        ```
        function Parent2(){
            this.name = "parent2";
            this.play = [1,2,3];
        }
        function Child2(){
            this.type = "child2";
        }
        Child2.prototype = new Parent2();
        //缺点：原型对象的属性是共享的
        ```
    3. 组合式继承
        ```
        function Parent3(){
            this.name = "parent3";
            this.play = [1,2,3];
        }
        function Child3(){
            Parent3.call(this);
            this.type = "child3";
        }
        Child3.prototype = Object.create(Parent3.prototype);
        Child3.prototype.constructor = Child3;
        ```

* js中深拷贝和浅拷贝：
    ```
    浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。
    深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。
    ```

* 什么是`闭包`、`作用域`、`原型`和`原型链`：
    1. 闭包：
        ```
        专业:当一个内部函数被其外部函数之外的变量引用时，就形成了一个闭包
        个人理解：在非定义作用域出执行的方法都会产生闭包 -- 涉及到词法作用域和静态作用域
        特性：
            1、所在作用域链中的参数和变量不会被垃圾回收机制回收 -- 也是缺点 会造成内存泄漏
            2、内部函数被外部作用域的变量引用。
        ```
    2. 作用域：一个function既是一个作用域
    3. 原型和原型链：
        ```
        1. 所有的对象都有__proto__属性，该属性对应该对象的原型.
        2. 所有的函数对象都有prototype属性，该属性的值会被赋值给该函数创建的对象的_proto_属性.
        3. 所有的原型对象都有constructor属性，该属性对应创建所有指向该原型的实例的构造函数.
        4. 函数对象和原型对象通过prototype和constructor属性进行相互关联.
        ```
    4. 拓展:（个人认知）
        ```
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

* [数组去重](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651550928&idx=1&sn=0912e56c7ccbb68cf36562e723e29da0&scene=1&srcid=0612ekFt6xkwpwwFhCDSPKnM#rd)
   ```
   ES6的解决方案：[...new Set(Arr)] -- 先转成set在拷贝数组
   普通方案：新建数组，对原数组进行循环遍历 -- 复杂度O(n^2)
   普通方案二：遍历数组，用object对象当做hash表。然后object.key()取出键
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

* [JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)


## 前端杂谈
* 说说HTML5中有趣的标签
    ```
    如果代码写的语义化，有利于SEO。搜索引擎就会很容易的读懂该网页要表达的意思
    ```

* 前端性能优化
    ```
    雪碧图，移动端响应式图片，静态资源CDN，减少Dom操作（事件代理、fragment），压缩JS和CSS、HTML等，DNS预解析
    ```

* 浏览器渲染原理
    <p align="center"><img src="image/webkitflow.png"></a></p>

    ```
    1、HTML被解析成DOM Tree，CSS被解析成CSS Rule Tree
    2、把DOM Tree和CSS Rule Tree经过整合生成Render Tree（布局阶段）
    3、元素按照算出来的规则，把元素放到它该出现的位置，通过显卡画到屏幕上
    ```

* script标签的`defer、async`的区别
    ```
    defer是在HTML解析完之后才会执行，如果是多个，按照加载的顺序依次执行
    async是在加载完成后立即执行，如果是多个，执行顺序和加载顺序无关
    ```

* 同源与跨域
    1. 什么是同源策略？
        ```
        限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。
        一个源指的是主机名、协议和端口号的组合，必须相同
        ```
    2. 跨域通信的几种方式
        ```
        JSONP | Hash | postMessage | WebSocket | CORS
        ```
    3. JSONP原理
        ```
        基本原理：利用script标签的异步加载特性实现给服务端传一个回调函数，服务器返回一个传递过去的回调函数名称
        的JS代码
        ```
    4. CORS跨域问题 [博客](http://newhtml.net/using-cors/)
        ```
        默认的CORS请求不携带cookies
        response:
            Access-Control-Allow-Origin（必选）：* 或者域名数组
            Access-Control-Allow-Credentials（可选）：true -- 配合前端withCredentials
            Access-Control-Allow-Headers（可选）-- 允许的请求头
        request:
            withCredentials ：true -- 让请求携带cookie
        ```

* 如何进行错误监控
    1. 前端错误的分类
        ```
        即时运行错误（代码错误）
        资源加载错误
        ```
    2. 错误的捕获方式:即时运行错误的捕获方式：
       ```
       try...catch
       window.onerror
       ```
    3. 资源加载错误：
        ```
        object.onerror（如img,script）
        performance.getEntries()
        Error事件捕获
        ```
    4. 延伸:跨域的js运行错误怎么捕获吗，错误提示什么，应该怎么处理？
        ```
        Script error
        1.在script标签增加crossorigin属性
        2.设置js资源响应头Access-Control-Allow-Orgin:*
        ```

* 代码优化基本方法
    1. 减少HTTP请求
    2. HTML优化：
        ```
        1、使用语义化标签
        2、减少iframe：iframe是SEO的大忌，iframe有好处也有弊端
        3、避免重定向
        ```
    3. CSS优化
        ```
        1、布局代码写前面
        2、删除空样式
        3、不滥用浮动，字体，需要加载的网络字体根据网站需求再添加
        4、选择器性能优化
        5、避免使用表达式，避免用id写样式
        ```
    4. js优化
        ```
        1、压缩
        2、减少重复代码
        ```
    5. 图片优化
        ```
        1、使用WebP
        2、图片合并，CSS sprite技术
        ```
    6. 减少DOM操作
        ```
        1、缓存已经访问过的元素
        2、"离线"更新节点, 再将它们添加到树中
        3、避免使用 JavaScript 输出页面布局--应该是 CSS 的事儿
        ```
    7. 使用JSON格式来进行数据交换
    8. 使用CDN加速
    9. 使用HTTP缓存：添加 `Expires` 或 `Cache-Control` 信息头
    10. 使用DNS预解析
        ```
        Chrome内置了DNS Prefetching技术, Firefox 3.5 也引入了这一特性，
        由于Chrome和Firefox 3.5本身对DNS预解析做了相应优化设置，
        所以设置DNS预解析的不良影响之一就是可能会降低Google Chrome浏览器及火狐Firefox 3.5浏览器的用户体验。
        预解析的实现：
            1、用meta信息来告知浏览器, 当前页面要做DNS预解析:
                <meta http-equiv="x-dns-prefetch-control" content="on" />
            2、在页面header中使用link标签来强制对DNS预解析:
                <link rel="dns-prefetch" href="http://bdimg.share.baidu.com" />
        ```

* DOM事件类

* 重排`Reflow`和重绘`Repaint` -- `重排一定触发重绘，但是重绘不一定触发重排。`
[如何写出高性能DOM](http://34585f3f.wiz03.com/share/s/0Qm5Y_0RRQtc2F-3Zy2piy1K0E4QKp0IAQvZ2PEFvB08u3fM)
    1. 重排
        ```
        对于DOM结构中的各个元素都有自己的盒子模型，这些都需要浏览器根据各种样式（浏览器的、开发人员定义的）
        来计算并根据计算结果将元素放到它该出现的位置，这个过程称为回流。
        - 触发条件：DOM的删除
        ```
    2. 重绘
        ```
        当各种盒子的位置、大小以及其他属性，例如颜色、字体大小等都确定下来后，浏览器于是便把这些元素都按照各
        自的特性绘制了一遍，于是页面的内容出现了，这个过程称之为重绘。
        ```
    3. 如何避免触发重拍和重绘
        ```
        1、删除dom，在完成修改后再把元素放回原来的位置
        2、多个dom创建可以先使用documentFragment创建完成在一次性加入到document
        ```

* HTTPS的握手过程
    ```
    1、浏览器将自己支持的一套加密规则发送给服务器。
    2、服务器从中选出一组加密算法与HASH算法，并将自己的身份信息以证书的形式发回给浏览器。证书里面包含了网站地址，
    加密公钥，以及证书的颁发机构等信息。
    3、浏览器获得网站证书之后浏览器要做以下工作：
        a.验证证书的合法
        b.如果证书受信任，或者是用户接受了不受信的证书，浏览器会生成一串随机数的密码，并用证书中提供的公钥加密。
        c.使用约定好的HASH算法计算握手消息，并使用生成的随机数对消息进行加密，最后将之前生成的所有信息发送给服务器
    4、网站接收浏览器发来的数据之后要做以下的操作：
        a.使用自己的私钥将信息解密取出密码，使用密码解密浏览器发来的握手消息，并验证HASH是否与浏览器发来的一致。
        b.使用密码加密一段握手消息，发送给浏览器。
    5、浏览器解密并计算握手消息的HASH，如果与服务端发来的HASH一致，此时握手过程结束，之后所有的通信数据将由之前
    浏览器生成的随机密码并利用对称加密算法进行加密。
    ```

* UTF-8和Unicode的区别
    ```
    UTF-8就是在互联网上使用最广的一种unicode的实现方式。
    Unicode的出现是为了统一地区性文字编码方案，为解决unicode如何在网络上传输的问题，于是面向传输的众多 UTF
    （UCS Transfer Format）标准出现了，顾名思义，UTF-8就是每次8个位传输数据，而UTF-16就是每次16个位。
    ASCII --> 地区性编码（GBK） --> Unicode --> UTF-8
    ```

## CSS
* `box-sizing`
    ```
    box-sizing属性可以为三个值之一：
        1、content-box，默认值，border和padding不计算入width之内
        2、padding-box，padding计算入width内
        3、border-box，border和padding计算入width之内
    ```

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
        2、避免使用eval、new Function等执行字符串的方法，除非确定字符串和用户输入无关
        3、使用cookie的httpOnly属性，加上了这个属性的cookie字段，js是无法进行读写的
        4、使用innerHTML、document.write的时候，如果数据是用户输入的，那么需要对象关键字符进行过滤与转义
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
   1. [pReact](https://zhuanlan.zhihu.com/p/30796007)
   2. [inferno](http://infernojs.org)
* Angular1.x


