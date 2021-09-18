# http面试题

## 1. session 和 cookie 有什么区别？

https://www.cnblogs.com/ityouknow/p/10856177.html

- Cookie: HTTP Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为可能
  - 主要用于会话状态管理、个性化设置、浏览器行为跟踪等
- Session: Session 代表着服务器和客户端一次会话的过程。Session对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的Web页面之间跳转时，存储在Session对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当客户端关闭会话，或者Session超时失效时会话结束。
- Cookie和Session的不同
  - 作用范围：Cookie保存在客户端（浏览器），Session保存在服务器
  - 存取方式：Cookie只能保存ASCII，Session可以存任意数据类型
  - 有效期：Cookie可设置为长时间保持，例如默认登录；Session失效时间较短，客户端关闭或者Session超时都会失效
  - 隐私策略：Cookie存储在客户端，比较容易遭到不法窃取；Session存储在服务端，安全性相对Cookie要好一些。
  - 存储大小不同：单个Cookie保存数据不能超过4K，Session可存储数据远高于Cookie
- Cookie和Session的关联
  - 浏览器没有状态（HTTP协议无状态），Cookie和Session可以借助SessionID配合识别客户端请求。
    - 客户端第一次向服务端发送请求时，服务端创建Session，并将SessionID返回给客户端浏览器；随后客户端将SessionID存入Cookie，Cookie也记录SessionID属于哪一个域名
    - 客户端第二次向服务器发送请求时，自动判断该域名是否存在Cookie信息，如果存在，则将Cookie信息一并发送给服务端，服务端从中获取SessionID继而查找对应Session信息，如果找到了仍然有效的Session，就继续这个Session，不然的话新开一个Session
- 浏览器禁止Cookie如何保障以上机制
  - 每次请求都携带SessionID以便查找
  - 使用Token机制
    - Token 的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。
    - 当用户第一次登录后，服务器根据提交的用户信息生成一个 Token，响应时将 Token 返回给客户端，以后客户端只需带上这个 Token 前来请求数据即可，无需再次登录验证。
- 如何考虑分布式Session问题？用户在 A 服务器登录，第二次请求跑到B服务器，会出现登录失效问题
  - 使用Nginx中的ip_hash策略：保证同一个IP地址会固定访问某一个后台服务器
  - 使用广播机制，将某个服务器上一个Session的变化广播给所有其他服务器
  - （建议）共享Session，将用户的Session等信息使用缓存中间件来统一管理，保障分发到每一个服务器的响应结果都一致。
- 跨域请求及Jsonp跨域
  - JSONP 的理念就是，与服务端约定好一个回调函数名，服务端接收到请求后，将返回一段 Javascript，在这段 Javascript 代码中调用了约定好的回调函数，并且将数据作为参数进行传递。当网页接收到这段 Javascript 代码后，就会执行这个回调函数，这时数据已经成功传输到客户端了。
  - JSONP 的缺点是：它只支持 GET 请求，而不支持 POST 请求等其他类型的 HTTP 请求。

