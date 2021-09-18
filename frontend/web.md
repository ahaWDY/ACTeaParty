**1. 什么是服务端渲染（SSR）？**

（1）什么是浏览器端渲染 (CSR)？
CSR是Client Side Render简称；页面上的内容是我们加载的js文件渲染出来的，js文件运行在浏览器上面，服务端只返回一个html模板。
过程 :
用户输入地址，客户端向服务器发送请求
=> 服务器传给浏览器相应的网页文件
=> 浏览器解析文件
=> 遇到ajax请求则向服务器再次请求一些数据
=> 服务器再次向浏览器发送相应的数据
=> 浏览器拿到ajax请求返回的数据后，将数据渲染在页面上

- 优点
可以向用户快速展示页面的内容，增加用户体验
给别人爬虫爬取相应的内容增加一定的困难
- 缺点
可能需要向服务器请求多次数据
不利于SEO 搜索引擎优化，即百度、搜狗等搜索引擎搜索不到客户端渲染的数据

（2）什么是服务器端渲染 (SSR)？
SSR是Server Side Render简称；页面上的内容是通过服务端渲染生成的，浏览器直接显示服务端返回的html就可以了。
过程：
客户端向服务器发送一次请求
=> 服务器接收请求，并在服务端操作网页文件，将对应数据导入文件
=> 服务器在服务端渲染好整个网页，发送给客户端
=> 客户端接收服务器发送过来的网页文件，不需要做任何操作，直接呈现

- 优点
只需要向服务器请求一次
利于SEO 搜索引擎优化，即能被搜索引擎搜索到，能向用户展示你网页的东西
- 缺点
如果数据量过大，在服务器渲染的时间就会过长，造成浏览器暂时的空白
容易被爬虫爬取




reference: 
https://zhuanlan.zhihu.com/p/90746589
https://cloud.tencent.com/developer/article/1782140



**2. 场景题：淘宝想要统计整个当前页面html文档的不同标签数量？你会怎么设计，怎么做？**
循环递归遍历整个页面，然后用document.getElementByTagName('*')来获取标签的名字，放进字典里，统计数量。
referennce: nowcoder.com/discuss/734451?type=post&order=time&pos=&page=1&ncTraceId=&channel=-1&source_id=search_post_nctrack



**3. 遇到跨域的问题前端是怎么解决的？**

1、 通过jsonp跨域
2、 document.domain + iframe跨域
3、 location.hash + iframe
4、 window.name + iframe跨域
5、 postMessage跨域
6、 跨域资源共享（CORS）
7、 nginx代理跨域
8、 nodejs中间件代理跨域
9、 WebSocket协议跨域

reference: https://segmentfault.com/a/1190000011145364
