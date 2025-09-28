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
    ```markdown
    1.创建了一个新对象
    2.将this指向这个新对象
    3.执行构造函数里面的代码
    4.返回新对象（this）
    ```

* js 实现继承的方式
    1. 借用构造函数实现继承
        ```javascript
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
        ```javascript
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
        ```javascript
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
        ```markdown
        专业:当一个内部函数被其外部函数之外的变量引用时，就形成了一个闭包
        个人理解：在非定义作用域出执行的方法都会产生闭包 -- 涉及到词法作用域和静态作用域
        特性：
            1. 所在作用域链中的参数和变量不会被垃圾回收机制回收 -- 也是缺点 会造成内存泄漏
            2. 内部函数被外部作用域的变量引用。
        ```
    2. 作用域：一个function既是一个作用域
    3. 原型和原型链：
        ```markdown
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
* JavaScript中的`macrotask`(宏任务)和`microtask`(微任务)
    1. `宏任务`
        ```
        是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）
        包括script整体代码，
            setTimeout、
            setInterval、
            I/O、UI交互事件、
            postMessage、
            MessageChannel、
            setImmediate(Node.js 环境)
        ```
    2.微任务
    ```
      是在当前 task 执行结束后立即执行的任务**。也就是说，在当前task任务后，下一个task之前，在渲染之前。
      所以它的响应速度相比setTimeout（setTimeout是task）会更快，因为无需等渲染。
      也就是说，在某一个macrotask执行完后，就会将在它执行期间产生的所有microtask都执行完毕（在渲染前）。
      microtask主要包含：
        Promise.then、
        MutaionObserver、
        process.nextTick(Node.js 环境)
    ```
* JavaScript任务运行机制

    ![avatar](image/task.jpeg)
    
* promise中几点注意事项
    1. promise构造函数是同步执行的，then方法是异步执行的

* [JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)

* WebSocket
    1. 原理：
        1. WebSocket协议基于TCP协议实现，包含初始的握手过程，以及后续的多次数据帧双向传输过程。
    2. 优点
        1. 通过第一次HTTP Request建立了连接之后，后续的数据交换都不用再重新发送HTTP Request，节省了带宽资源
        2. WebSocket的连接是双向通信的连接，在同一个TCP连接上，既可以发送，也可以接收
        3. 具有多路复用的功能(multiplexing)，也即几个不同的URI可以复用同一个WebSocket连接

## 前端杂谈
* 说说HTML5中有趣的标签
    ```
    如果代码写的语义化，有利于SEO。搜索引擎就会很容易的读懂该网页要表达的意思
    ```
* 前端性能优化
    ```
    雪碧图，
    移动端响应式图片，
    静态资源CDN，
    减少Dom操作（事件代理、fragment），
    压缩JS和CSS、HTML等，
    DNS预解析
    ```
* 浏览器渲染原理
    ![avatar](image/webkitflow.png)

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
    11. 提升LCP（最大内容绘制）的优化策略：
        1. 优化图片和视频：
            * 压缩图片：减小图片文件大小，提高加载速度。
            使用现代图片格式：例如WebP，比传统格式（如JPEG）更小。
            * 实现懒加载：只加载用户滚动到可见区域的内容，延迟加载非关键图片和视频。
            * 使用响应式图片：根据设备和屏幕尺寸提供不同大小的图片，避免加载不必要的大文件。
        2. 优化关键资源和渲染路径：
            * 优先加载关键资源：使用 <link rel="preload"> 预加载影响LCP的关键图片或字体，让浏览器尽早请求这些资源。
            * 内联关键CSS：:将首屏渲染所需的最少CSS内联到HTML中，让页面快速可见。
            * 优化字体加载：采用更优的字体加载策略，减少字体文件对渲染的阻塞。
        3. 减少服务器响应时间（TTFB）：
            * 使用CDN（内容分发网络）：加速静态资源的加载，缩短用户与服务器的距离。
            * 优化服务器性能：提高服务器处理请求的速度，缩短第一次接收到HTML文档的时间。
            * 合理使用缓存：确保缓存设置得当，避免不必要的数据传输，减少TTFB。
        4. 管理第三方脚本：
            * 移除或延迟非必要的第三方脚本：减少对核心内容加载的阻塞。
            * 谨慎集成第三方服务：例如广告、分析工具等，注意它们的加载和执行是否会影响页面性能。
    12. 提升FCP（首次内容绘制）的优化策略：
        1. 缩短TTFB：这是实现良好FCP和LCP的基础，因为TTFB过高可能导致浏览器下载大量阻止渲染的资源。
        2. 避免服务器重定向：减少服务器重定向的数量，确保用户直接访问最终目标。
        3. 优化JavaScript：异步加载JavaScript，避免其阻塞页面渲染。
        4. 减少DOM 操作：优化页面结构，减少不必要的DOM节点，可以帮助浏览器更快地渲染内容。

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

* TCP的三次握手和四次挥手 [TCP的概述](https://juejin.im/post/5c078058f265da611c26c235)
    ![avatar](image/tcp3-4.png)

* http2.0相比http1.0的区别
    ```
    在 HTTP/1 中，每次请求都会建立一次HTTP连接，也就是我们常说的3次握手4次挥手，这个过程在一次请求过程中占用了相当长的时间，即使开启了 Keep-Alive ，解决了多次连接的问题，但是依然有两个效率上的问题：
    * 第一个：串行的文件传输。当请求a文件时，b文件只能等待，等待a连接到服务器、服务器处理文件、服务器返回文件，这三个步骤。我们假设这三步用时都是1秒，那么a文件用时为3秒，b文件传输完成用时为6秒，依此类推。（注：此项计算有一个前提条件，就是浏览器和服务器是单通道传输）
    * 第二个：连接数过多。我们假设Apache设置了最大并发数为300，因为浏览器限制，浏览器发起的最大请求数为6，也就是服务器能承载的最高并发为50，当第51个人访问时，就需要等待前面某个请求处理完成。
    HTTP/2的多路复用就是为了解决上述的两个性能问题。
    在 HTTP/2 中，有两个非常重要的概念，分别是帧（frame）和流（stream）。
    帧代表着最小的数据单位，每个帧会标识出该帧属于哪个流，流也就是多个帧组成的数据流。
    多路复用，就是在一个 TCP 连接中可以存在多条流。换句话说，也就是可以发送多个请求，对端可以通过帧中的标识知道属于哪个请求。通过这个技术，可以避免 HTTP 旧版本中的队头阻塞问题，极大的提高传输性能。
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
* XSS漏洞:跨站脚本攻击 [Cross-Site Scripting](https://en.wikipedia.org/wiki/Cross-site_scripting)
    > 通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序
    ```
    解决办法:
        1、在不同上下文中，使用合适的 escape 方式
        2、避免使用eval、new Function等执行字符串的方法，除非确定字符串和用户输入无关
        3、使用cookie的httpOnly属性，加上了这个属性的cookie字段，js是无法进行读写的
        4、使用innerHTML、document.write的时候，如果数据是用户输入的，那么需要对象关键字符进行过滤与转义
    ```
* CSRF漏洞:跨站点请求伪造 [Cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
    * 检测：抓取一个正常请求的数据包，去掉Referer字段后再重新提交，如果该提交还有效，那么基本上可以确定存在CSRF漏洞
    ```
    解决办法:
        1、给所有请求加上 token 检查。token 一般是随机字符串，只需确保其不可预测性即可 --无法保证token的安全性
        2、验证 HTTP Referer 字段 -- 无法防御自身的攻击
        3、在 HTTP 头中自定义属性并验证 -- 需要修改所有请求为XMLHttpRequest，修改代价巨大
    ```



