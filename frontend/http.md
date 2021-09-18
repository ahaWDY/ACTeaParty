# HTTP面试题

## 1. 如何用ajax原生实现一个post请求？

```js
var data = {
  city: city,
  area: area,
}
var data = 'city='+city+'area'+area;
var XHR = null;
if(window.XMLHttpRequest){
  //非IE内核
  XHR = new XMLHttpRequest();
}else if(window.ActiveXoject){
  //IE内核
  XHR = null;
}

if(XHR){
	XHR.open("POST","../plus/ddidy.php",true);
  XHR.onreadystatechange = function (){
    //readyState值说明
    //0.初始化，XHR对象已经创建，还未执行open
    //1.载入，已经调用open方法，但还未发送请求
    //2.载入完成，请求已经发送完成
    //3.交互，可以接收到部分数据
    
    //status值说明
    //200:成功
    //404:没有发现文件、查询或URL
    //500:服务器产生内部错误
    if(XHR.readyState == 4 && XHR.status == 200){
      //这里可以对返回的内容做处理
      //一般会返回json或者xml数据格式
      console.log(XHR.responseText);
      //主动释放，JS本身也会回收的
      XHR=null;
    }
  };
  XHR.setRequestHeader("Content-type","application/x-www-form-urlencoded");
  XHR.send(data);
}
```

参考：

https://blog.csdn.net/qiphon3650/article/details/77968224

https://blog.csdn.net/weixin_30607659/article/details/96514887





## 2. 说一下 Http 缓存策略，有什么区别，分别解决了什么问题

**1）浏览器缓存策略**

浏览器每次发起请求时，先在本地缓存中查找结果以及缓存标识，根据缓存标识来判断是否使用本地缓存。如果缓存有效，则使
用本地缓存；否则，则向服务器发起请求并携带缓存标识。根据是否需向服务器发起HTTP请求，将缓存过程划分为两个部分：
强制缓存和协商缓存，强缓优先于协商缓存。

- 强缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行比较缓存策略。
- 协商缓存，让客户端与服务器之间能实现缓存文件是否更新的验证、提升缓存的复用率，将缓存信息中的Etag和Last-Modified
  通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存。

HTTP缓存都是从第二次请求开始的：

- 第一次请求资源时，服务器返回资源，并在response header中回传资源的缓存策略；
- 第二次请求时，浏览器判断这些请求参数，击中强缓存就直接200，否则就把请求参数加到request header头中传给服务器，看是否击中协商缓存，击中则返回304，否则服务器会返回新的资源。这是缓存运作的一个整体流程图：

[![img](https://camo.githubusercontent.com/df822872ee2a8aef44c665f8fffd13c4cc4eb637bd8706ce4899e8eb72d2a431/687474703a2f2f696d672d7374617469632e796964656e6778756574616e672e636f6d2f77786170702f69737375652d696d672f7169642d382e706e67)](https://camo.githubusercontent.com/df822872ee2a8aef44c665f8fffd13c4cc4eb637bd8706ce4899e8eb72d2a431/687474703a2f2f696d672d7374617469632e796964656e6778756574616e672e636f6d2f77786170702f69737375652d696d672f7169642d382e706e67)

**2）强缓存**

- 强缓存命中则直接读取浏览器本地的资源，在network中显示的是from memory或者from disk
- 控制强制缓存的字段有：Cache-Control（http1.1）和Expires（http1.0）
- Cache-control是一个相对时间，用以表达自上次请求正确的资源之后的多少秒的时间段内缓存有效。
- Expires是一个绝对时间。用以表达在这个时间点之前发起请求可以直接从浏览器中读取数据，而无需发起请求
- Cache-Control的优先级比Expires的优先级高。前者的出现是为了解决Expires在浏览器时间被手动更改导致缓存判断错误的问题。
  如果同时存在则使用Cache-control。

**3）强缓存-expires**

- 该字段是服务器响应消息头字段，告诉浏览器在过期时间之前可以直接从浏览器缓存中存取数据。
- Expires 是 HTTP 1.0 的字段，表示缓存到期时间，是一个绝对的时间 (当前时间+缓存时间)。在响应消息头中，设置这个字段之后，就可以告诉浏览器，在未过期之前不需要再次请求。
- 由于是绝对时间，用户可能会将客户端本地的时间进行修改，而导致浏览器判断缓存失效，重新请求该资源。此外，即使不考虑修改，时差或者误差等因素也可能造成客户端与服务端的时间不一致，致使缓存失效。
- 优势特点
  - 1、HTTP 1.0 产物，可以在HTTP 1.0和1.1中使用，简单易用。
  - 2、以时刻标识失效时间。
- 劣势问题
  - 1、时间是由服务器发送的(UTC)，如果服务器时间和客户端时间存在不一致，可能会出现问题。
  - 2、存在版本问题，到期之前的修改客户端是不可知的。

**4）强缓存-cache-control**

- 已知Expires的缺点之后，在HTTP/1.1中，增加了一个字段Cache-control，该字段表示资源缓存的最大有效时间，在该时间内，客户端不需要向服务器发送请求。

- 这两者的区别就是前者是绝对时间，而后者是相对时间。下面列举一些 `Cache-control` 字段常用的值：(完整的列表可以查看MDN)

  - `max-age`：即最大有效时间。
  - `must-revalidate`：如果超过了 `max-age` 的时间，浏览器必须向服务器发送请求，验证资源是否还有效。
  - `no-cache`：不使用强缓存，需要与服务器验证缓存是否新鲜。
  - `no-store`: 真正意义上的“不要缓存”。所有内容都不走缓存，包括强制和对比。
  - `public`：所有的内容都可以被缓存 (包括客户端和代理服务器， 如 CDN)
  - `private`：所有的内容只有客户端才可以缓存，代理服务器不能缓存。默认值。

- **Cache-control 的优先级高于 Expires**，为了兼容 HTTP/1.0 和 HTTP/1.1，实际项目中两个字段都可以设置。

- 该字段可以在请求头或者响应头设置，可组合使用多种指令：

  - 可缓存性

    ：

    - public：浏览器和缓存服务器都可以缓存页面信息
    - private：default，代理服务器不可缓存，只能被单个用户缓存
    - no-cache：浏览器器和服务器都不应该缓存页面信息，但仍可缓存，只是在缓存前需要向服务器确认资源是否被更改。可配合private，
      过期时间设置为过去时间。
    - only-if-cache：客户端只接受已缓存的响应

  - 到期

    - max-age=：缓存存储的最大周期，超过这个周期被认为过期。
    - s-maxage=：设置共享缓存，比如can。会覆盖max-age和expires。
    - max-stale[=]：客户端愿意接收一个已经过期的资源
    - min-fresh=：客户端希望在指定的时间内获取最新的响应
    - stale-while-revalidate=：客户端愿意接收陈旧的响应，并且在后台一部检查新的响应。时间代表客户端愿意接收陈旧响应
      的时间长度。
    - stale-if-error=：如新的检测失败，客户端则愿意接收陈旧的响应，时间代表等待时间。

  - 重新验证和重新加载

    - must-revalidate：如页面过期，则去服务器进行获取。
    - proxy-revalidate：用于共享缓存。
    - immutable：响应正文不随时间改变。

  - 其他

    - no-store：绝对禁止缓存
    - no-transform：不得对资源进行转换和转变。例如，不得对图像格式进行转换。

- 优势特点

  - 1、HTTP 1.1 产物，以时间间隔标识失效时间，解决了Expires服务器和客户端相对时间的问题。
  - 2、比Expires多了很多选项设置。

- 劣势问题

  - 1、存在版本问题，到期之前的修改客户端是不可知的。

**5）协商缓存**

- 协商缓存的状态码由服务器决策返回200或者304
- 当浏览器的强缓存失效的时候或者请求头中设置了不走强缓存，并且在请求头中设置了If-Modified-Since 或者 If-None-Match 的时候，会将这两个属性值到服务端去验证是否命中协商缓存，如果命中了协商缓存，会返回 304 状态，加载浏览器缓存，并且响应头会设置 Last-Modified 或者 ETag 属性。
- 对比缓存在请求数上和没有缓存是一致的，但如果是 304 的话，返回的仅仅是一个状态码而已，并没有实际的文件内容，因此 在响应体体积上的节省是它的优化点。
- 协商缓存有 2 组字段(不是两个)，控制协商缓存的字段有：Last-Modified/If-Modified-since（http1.0）和Etag/If-None-match（http1.1）
- Last-Modified/If-Modified-since表示的是服务器的资源最后一次修改的时间；Etag/If-None-match表示的是服务器资源的唯一标
  识，只要资源变化，Etag就会重新生成。
- Etag/If-None-match的优先级比Last-Modified/If-Modified-since高。

**6）协商缓存-协商缓存-Last-Modified/If-Modified-since**

- 1.服务器通过 `Last-Modified` 字段告知客户端，资源最后一次被修改的时间，例如 `Last-Modified: Mon, 10 Nov 2018 09:10:11 GMT`
- 2.浏览器将这个值和内容一起记录在缓存数据库中。
- 3.下一次请求相同资源时时，浏览器从自己的缓存中找出“不确定是否过期的”缓存。因此在请求头中将上次的 `Last-Modified` 的值写入到请求头的 `If-Modified-Since` 字段
- 4.服务器会将 `If-Modified-Since` 的值与 `Last-Modified` 字段进行对比。如果相等，则表示未修改，响应 304；反之，则表示修改了，响应 200 状态码，并返回数据。
- 优势特点
  - 1、不存在版本问题，每次请求都会去服务器进行校验。服务器对比最后修改时间如果相同则返回304，不同返回200以及资源内容。
- 劣势问题
  - 2、只要资源修改，无论内容是否发生实质性的变化，都会将该资源返回客户端。例如周期性重写，这种情况下该资源包含的数据实际上一样的。
  - 3、以时刻作为标识，无法识别一秒内进行多次修改的情况。 如果资源更新的速度是秒以下单位，那么该缓存是不能被使用的，因为它的时间单位最低是秒。
  - 4、某些服务器不能精确的得到文件的最后修改时间。
  - 5、如果文件是通过服务器动态生成的，那么该方法的更新时间永远是生成的时间，尽管文件可能没有变化，所以起不到缓存的作用。

**7）协商缓存-Etag/If-None-match**

- 为了解决上述问题，出现了一组新的字段 `Etag` 和 `If-None-Match`
- `Etag` 存储的是文件的特殊标识(一般都是 hash 生成的)，服务器存储着文件的 `Etag` 字段。之后的流程和 `Last-Modified` 一致，只是 `Last-Modified` 字段和它所表示的更新时间改变成了 `Etag` 字段和它所表示的文件 hash，把 `If-Modified-Since` 变成了 `If-None-Match`。服务器同样进行比较，命中返回 304, 不命中返回新资源和 200。
- 浏览器在发起请求时，服务器返回在Response header中返回请求资源的唯一标识。在下一次请求时，会将上一次返回的Etag值赋值给If-No-Matched并添加在Request Header中。服务器将浏览器传来的if-no-matched跟自己的本地的资源的ETag做对比，如果匹配，则返回304通知浏览器读取本地缓存，否则返回200和更新后的资源。
- **Etag 的优先级高于 Last-Modified**。
- 优势特点
  - 1、可以更加精确的判断资源是否被修改，可以识别一秒内多次修改的情况。
  - 2、不存在版本问题，每次请求都回去服务器进行校验。
- 劣势问题
  - 1、计算ETag值需要性能损耗。
  - 2、分布式服务器存储的情况下，计算ETag的算法如果不一样，会导致浏览器从一台服务器上获得页面内容后到另外一台服务器上进行验证时现ETag不匹配的情况。

Reference: https://github.com/lgwebdream/FE-Interview/issues/14



## 3. GET和POST的请求的区别

`GET`：**获取资源**，用来请求访问已被URI（统一资源标志符，和URL是包含和被包含的关系）识别的资源。
`POST`：用来**传输实体的主体**，虽然GET也可以实现，但是一般不用。

**请求参数**：GET请求参数是通过URL传递的，多个参数以&连接，POST请求放在request body中。
**请求缓存**：GET请求会被缓存，而POST请求不会，除非手动设置。
**收藏为书签**：GET请求支持，POST请求不支持。
**安全性**：POST比GET安全，GET请求在浏览器回退时是无害的，而POST会再次请求。
**历史记录**：GET请求参数会被完整保留在浏览历史记录里，而POST中的参数不会被保留。
**编码方式**：GET请求只能进行url编码，而POST支持多种编码方式。
**对参数的数据类型**：GET只接受ASCII字符，而POST没有限制。

补充：通过浏览器地址栏输入URL访问资源的方式都是`GET`请求。

Reference: https://segmentfault.com/a/1190000023940344



## 4. POST和PUT请求的区别

POST方法和PUT方法请求最根本的区别是发起请求的目的不同。

post 请求的目的是根据资源自身的语义来处理这个资源。 后一个请求不会把第一个请求覆盖掉，所以Post可以用来增加资源。

put请求的目的是用来替换整个目标资源。put 请求具有 **幂等性（idempotent）**，也就是“同样的请求，不管发多少次，每次服务器处理完之后的结果，都和只发一次是一样的。” 如果两个请求相同，后一个请求会把第一个请求覆盖掉，所以PUT可以用来改资源。



另外在缓存性上，post 的响应是可以被缓存的，put 却不行。



reference：https://www.huaweicloud.com/articles/ca8109ea722db9de54ec224084e30747.html

https://zhuanlan.zhihu.com/p/58516651



## 5. websocket如何握手建立连接？

WebSocket protocol 是HTML5一种新的协议。它实现了浏览器与服务器全双工通信(full-duplex)。一开始的握手需要借助HTTP请求完成。

### 目的：即时通讯，替代轮询

网站上的即时通讯是很常见的，比如网页的QQ，聊天系统等。按照以往的技术能力通常是采用**轮询、Comet**技术解决。

HTTP协议是非持久化的，单向的网络协议，在建立连接后只允许浏览器向服务器发出请求后，服务器才能返回相应的数据。当需要即时通讯时，通过轮询在特定的时间间隔（如1秒），由浏览器向服务器发送Request请求，然后将最新的数据返回给浏览器。这样的方法最明显的缺点就是需要不断的发送请求，而且通常HTTP request的Header是非常长的，为了传输一个很小的数据 需要付出巨大的代价，是很不合算的，占用了很多的宽带。

缺点：会导致过多不必要的请求，浪费流量和服务器资源，每一次请求、应答，都浪费了一定流量在相同的头部信息上

然而WebSocket的出现可以弥补这一缺点。在WebSocket中，只需要服务器和浏览器通过HTTP协议进行一个握手的动作，然后单独建立一条TCP的通信通道进行数据的传送。

- 原理

WebSocket同HTTP一样也是应用层的协议，但是它是一种**双向通信协议**，是建立在TCP之上的。

- 连接过程 —— 握手过程

- 1. 浏览器、服务器建立TCP连接，三次握手。这是通信的基础，传输控制层，若失败后续都不执行。

     

  2. TCP连接成功后，浏览器通过HTTP协议向服务器传送WebSocket支持的版本号等信息。（**开始前的HTTP握手**）

     

  3. 服务器收到客户端的握手请求后，**同样采用HTTP协议**回馈数据。

     

  4. 当收到了连接成功的消息后，通过TCP通道进行传输通信。

- WebSocket与HTTP的关系
- 相同点

- 1. 都是一样基于TCP的，都是可靠性传输协议。

  2. 都是应用层协议。

- 不同点

  1. WebSocket是双向通信协议，模拟Socket协议，可以双向发送或接受信息。HTTP是单向的。

  2. WebSocket是需要握手进行建立连接的。


- 联系

WebSocket在建立握手时，数据是通过HTTP传输的。但是建立之后，在真正传输时候是不需要HTTP协议的。

- WebSocket与Socket的关系

Socket其实并不是一个协议，而是为了方便使用TCP或UDP而抽象出来的一层，是位于应用层和传输控制层之间的一组接口。

> Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

当两台主机通信时，必须通过Socket连接，Socket则利用TCP/IP协议建立TCP连接。TCP连接则更依靠于底层的IP协议，IP协议的连接则依赖于链路层等更低层次。

WebSocket则是一个典型的应用层协议。

- 区别

**Socket是传输控制层协议，WebSocket是应用层协议。**

- WebSocket 机制

以下简要介绍一下 WebSocket 的原理及运行机制。

WebSocket 是 HTML5 一种新的协议。它实现了浏览器与服务器全双工通信，能更好的节省服务器资源和带宽并达到实时通讯，它建立在 TCP 之上，同 HTTP 一样通过 TCP 来传输数据，但是它和 HTTP 最大不同是：

- WebSocket 是一种双向通信协议，在建立连接后，WebSocket 服务器和 Browser/Client Agent 都能主动的向对方发送或接收数据，就像 Socket 一样；
- WebSocket 需要类似 TCP 的客户端和服务器端通过握手连接，连接成功后才能相互通信。

非 WebSocket 模式传统 HTTP 客户端与服务器的交互如下图所示：

##### 图 1. 传统 HTTP 请求响应客户端服务器交互图

![img](https://filescdn.proginn.com/38d79ac50bb3b7e2eb8040afaf5084c6/81104ee51b31e59990443b7f3def683a.webp)

图 1. 传统 HTTP 请求响应客户端服务器交互图

使用 WebSocket 模式客户端与服务器的交互如下图：

##### 图 2.WebSocket 请求响应客户端服务器交互图

![img](https://filescdn.proginn.com/33ea46524e9bf664f68bee0d35669ee6/2b83897ec988a3687649886b2dd2ceb0.webp)



图 2.WebSocket 请求响应客户端服务器交互图

上图对比可以看出，相对于传统 HTTP 每次请求-应答都需要客户端与服务端建立连接的模式，WebSocket 是类似 Socket 的 TCP 长连接的通讯模式，一旦 WebSocket 连接建立后，后续数据都以帧序列的形式传输。在客户端断开 WebSocket 连接或 Server 端断掉连接前，不需要客户端和服务端重新发起连接请求。在海量并发及客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽资源的消耗，有明显的性能优势，且客户端发送和接受消息是在同一个持久连接上发起，实时性优势明显。

我们再通过客户端和服务端交互的报文看一下 WebSocket 通讯与传统 HTTP 的不同：

在客户端，new WebSocket 实例化一个新的 WebSocket 客户端对象，连接类似 ws://yourdomain:port/path 的服务端 WebSocket URL，WebSocket 客户端对象会自动解析并识别为 WebSocket 请求，从而连接服务端端口，执行双方握手过程，客户端发送数据格式类似：

##### 清单 1.WebSocket 客户端连接报文

```
GET /webfin/websocket/ HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: xqBt3ImNzJbYqRINxEFlkg==
Origin: http://localhost:8080
Sec-WebSocket-Version: 13
```

可以看到，客户端发起的 WebSocket 连接报文类似传统 HTTP 报文，”Upgrade：websocket”参数值表明这是 WebSocket 类型请求，“Sec-WebSocket-Key”是 WebSocket 客户端发送的一个 base64 编码的密文，要求服务端必须返回一个对应加密的“Sec-WebSocket-Accept”应答，否则客户端会抛出“Error during WebSocket handshake”错误，并关闭连接。

服务端收到报文后返回的数据格式类似：

##### 清单 2.WebSocket 服务端响应报文

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: K7DJLdLooIwIG/MOpvWFB3y3FE8=
```

“Sec-WebSocket-Accept”的值是服务端采用与客户端一致的密钥计算出来后返回客户端的,“HTTP/1.1 101 Switching Protocols”表示服务端接受 WebSocket 协议的客户端连接，经过这样的请求-响应处理后，客户端服务端的 WebSocket 连接握手成功, 后续就可以进行 TCP 通讯了。

在开发方面，WebSocket API 也十分简单，我们只需要实例化 WebSocket，创建连接，然后服务端和客户端就可以相互发送和响应消息，在下文 WebSocket 实现及案例分析部分，可以看到详细的 WebSocket API 及代码实现。

Reference: https://jishuin.proginn.com/p/763bfbd32153



## 6. tcp的三次握手和四次挥手的过程?四次握手的时候如果有一方一直不发送关闭有什么弊端?拥塞控制?滑动窗口?快重传?


- TCP 的三次握手:

假设 A 为客户端，B 为服务器端。

首先 B 处于 LISTEN（监听）状态，等待客户的连接请求。

A 向 B 发送连接请求报文，SYN=1，ACK=0，选择一个初始的序号 x。

B 收到连接请求报文，如果同意建立连接，则向 A 发送连接确认报文，SYN=1，ACK=1，确认号为 x+1，同时也选择一个初始的序号 y。

A 收到 B 的连接确认报文后，还要向 B 发出确认，确认号为 y+1，序号为 x+1。

B 收到 A 的确认后，连接建立。

- TCP 的四次挥手:
以下描述不讨论序号和确认号，因为序号和确认号的规则比较简单。并且不讨论 ACK，因为 ACK 在连接建立之后都为 1。

A 发送连接释放报文，FIN=1。

B 收到之后发出确认，此时 TCP 属于半关闭状态，B 能向 A 发送数据但是 A 不能向 B 发送数据。

当 B 不再需要连接时，发送连接释放报文，FIN=1。

A 收到后发出确认，进入 TIME-WAIT 状态，等待 2 MSL（最大报文存活时间）后释放连接。

B 收到 A 的确认后释放连接。

- 如果有一方一直不发送关闭另一方就会一直发送确认报文，占用网络资源
- 拥塞控制/滑动窗口/快重传 详见博客

reference: https://blog.csdn.net/wardseptember/article/details/109055687


