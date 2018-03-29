### HTTP简介
> HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

> HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

### HTTP的特点
1. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
2. 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
3. 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
4. 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。
5. 支持B/S及C/S模式。

### URI和URL的区别
* URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源。
    > Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的
    * URI一般由三部组成：
        1. 访问资源的命名机制
        2. 存放资源的主机名
        3. 资源自身的名称，由路径表示，着重强调于资源。
* URL是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。
    > URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。
    * 采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL一般由三部组成：
        1. 协议(或称为服务方式)
        2. 存有该资源的主机IP地址(有时也包括端口号)
        3. 主机资源的具体地址。如目录和文件名等

* URN，uniform resource name，统一资源命名，是通过名字来标识资源，比如mailto:java-net@java.sun.com。
    > URI是以一种抽象的，高层次概念定义统一资源标识，而URL和URN则是具体的资源标识的方式。URL和URN都是一种URI。笼统地说，每个 URL 都是 URI，但不一定每个 URI 都是 URL。这是因为 URI 还包括一个子类，即统一资源名称 (URN)，它命名资源但不指定如何定位资源。上面的 mailto、news 和 isbn URI 都是 URN 的示例。



### HTTP之请求消息Request
1. 请求行（request line）
    ```
    GET /562f25980001b1b106000338.jpg HTTP/1.1
    POST / HTTP1.1
    ```
2. 请求头部（header）
3. 空行
4. 请求数据

GET

    ①   GET /562f25980001b1b106000338.jpg HTTP/1.1
    ②   Host    img.mukewang.com
         User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
         Accept    image/webp,image/*,*/*;q=0.8
         Referer    http://www.imooc.com/
         Accept-Encoding    gzip, deflate, sdch
         Accept-Language    zh-CN,zh;q=0.8
    ③
    ④

POST

    ①   POST / HTTP1.1
    ②   Host:www.wrox.com
         User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
         Content-Type:application/x-www-form-urlencoded
         Content-Length:40
         Connection: Keep-Alive
    ③
    ④   name=Professional%20Ajax&publisher=Wiley

### HTTP之状态码
> 状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

1. 1xx：指示信息--表示请求已接收，继续处理
2. 2xx：成功--表示请求已被成功接收、理解、接受
3. 3xx：重定向--要完成请求必须进行更进一步的操作
4. 4xx：客户端错误--请求有语法错误或请求无法实现
5. 5xx：服务器端错误--服务器未能实现合法的请求

常见状态码：[更多状态码](http://www.runoob.com/http/http-status-codes.html)

    200 OK                        //客户端请求成功
    400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
    401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
    403 Forbidden                 //服务器收到请求，但是拒绝提供服务
    404 Not Found                 //请求资源不存在，eg：输入了错误的URL
    500 Internal Server Error     //服务器发生不可预期的错误
    503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常


### HTTP请求方法
1. HTTP1.0定义了三种请求方法： `GET`, `POST` 和 `HEAD`方法。
2. HTTP1.1新增了五种请求方法：`OPTIONS`, `PUT`, `DELETE`, `TRACE` 和 `CONNECT` 方法。
3. HTTP2.0
    ```
    1. GET          请求指定的页面信息，并返回实体主体。
    2. HEAD         类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
    3. POST         向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
    4. PUT          从客户端向服务器传送的数据取代指定的文档的内容。
    5. DELETE       请求服务器删除指定的页面。
    6. CONNECT      HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
    7. OPTIONS      允许客户端查看服务器的性能。
    8. TRACE        回显服务器收到的请求，主要用于测试或诊断。
    ```
### HTTP工作原理
> HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。HTTP协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。服务器以一个状态行作为响应，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。

以下是 HTTP 请求/响应的步骤：
1. 客户端连接到Web服务器
一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如，http://www.oakcms.cn。
2. 发送HTTP请求
通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成。
3. 服务器接受请求并返回HTTP响应
Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。
4. 释放连接TCP连接
若connection 模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;
5. 客户端浏览器解析HTML内容
客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

    1. 浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;
    2. 解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立TCP连接;
    3. 浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 TCP 三次握手的第三个报文的数据发送给服务器;
    4. 服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;
    5. 释放 TCP连接;
    6. 浏览器将该 html 文本并显示内容;

### GET和POST请求的区别
1. 请求数据方式 -- **GET追加在URL之后。POST把数据放到HTTP包的包体中**
2. 数据大小 -- **GET限制于浏览器对URL长度的显示。POST理论上不受限，实际受限WEB服务器对POST数据的大小限制**
3. 安全性 -- **POST的安全性要比GET的安全性高**

### HTTP/2
* 二进制协议 -- **HTTP/1.1 版的头信息肯定是文本（ASCII编码），数据体可以是文本，也可以是二进制。HTTP/2 则是一个彻底的二进制协议，头信息和数据体都是二进制，并且统称为"帧"（frame）：头信息帧和数据帧**
* 多工 -- **HTTP/2 复用TCP连接，在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序一一对应，这样就避免了"队头堵塞"。**
* 数据流 --
* 头信息压缩
* 服务器推送 -- **HTTP/2 允许服务器未经请求，主动向客户端发送资源，这叫做服务器推送（server push）**