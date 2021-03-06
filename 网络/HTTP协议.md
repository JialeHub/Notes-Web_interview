# HTTP



## HTTP各个版本的区别

### HTTP 0.9

- 最简单的版本，只有get请求、没有请求头
- 1991

### HTTP 1.0

- HTTP0.9基础上加入请求头、版本号、状态码、Content-Type
- 一个HTTP请求需要建立一条TCP连接
- 缓存：只支持Expires、Last-Modified / If-Modified-Since
- 请求方法更多(POST、PUT、DELETE、HEAD)
- 1996

### HTTP 1.1

- **持久连接**（长连接、**Connection:keep-alive**、多个资源请求复用一条TCP连接，串行）
- 支持**断点续传**（头部有个Range表示下载范围）
- **缓存**：支持cache-Control、ETag / If-None-Match
- **Host**字段
- **请求方法**更多(OPTIONS、TRACE、CONNECT)
- 1999

### HTTP 2.0

- **多路复用**(并行，解决队头阻塞)
- **二进制分帧**
- **数据流传输**
- **头部压缩**
- **服务器Push推送**
- 2013

### HTTP 3.0

- 传输层改为**UDP**
- **QUIC**升级版(2015)
- 2018

|                                 | HTTP 1.0                                  | HTTP 1.1                           | HTTP 2.0      | HTTP 3.0 |
| ------------------------------- | ----------------------------------------- | ---------------------------------- | ------------- | -------- |
| 持久连接(Connection:Keep-Alive) | 否                                        | 是                                 |               |          |
| 断点续传                        | 否                                        | 是                                 |               |          |
| 缓存                            | If-Modified-Since、Last-Modified、Expires | Etag、If-None-Match、Cache-Control |               |          |
| host字段                        | 否                                        | 是                                 |               |          |
| 请求方法                        | 少                                        | 多                                 |               |          |
| 二进制分帧                      | 否                                        | 否                                 | 是            |          |
| 多路复用(解决队头堵塞)          | 否                                        | 否                                 | 是            |          |
| 数据流                          | 否                                        | 否                                 | 是            |          |
| 头部压缩                        | 否                                        | 否                                 | 是，HPACK算法 |          |
| 服务器推送                      | 否                                        | 否                                 | 是            |          |
| 传输层协议                      | TCP                                       | TCP                                | TCP           | UDP      |
| 年份                            | 1996                                      | 1999                               | 2013          | 2018     |



## HTTP报文

### HTTP请求(Request)报文

- **请求行**：**请求方法** 	**URI** 	**HTTP协议版本**

- **HTTP头**(通用信息头，请求头，实体头) 

- **空行**

- **请求报文主体**

  ![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/4/19/162db6358082ff05~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

  ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4fb5bb2cb1664850b52e32d57af74f2f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

### HTTP响应(Response)报文

- **状态行**：**版本号** 	**状态码**	 **原因**
- **HTTP头**(通用信息头，响应头，实体头)
- **空行**
- **响应报文主体**

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/4/19/162db635806ca887~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



## 状态码

- 1xx系列：信息状态码，客户端应相应的某些动作，代表请求已被接受，需要继续处理。
- 2xx系列：成功状态码，代表请求已成功被服务器接收、理解、并接受。
- 3xx系列：重定向状态码，代表需要客户端采取进一步的操作才能完成请求，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的 Location 域中指明。
- 4xx系列：客户端错误，表示请求错误。代表了客户端看起来可能发生了错误，妨碍了服务器的处理。
- 5xx系列：服务器错误，代表了服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。

| 状态码 | 含义           | 描述                                                         |
| ------ | -------------- | ------------------------------------------------------------ |
| 100    | 继续           | 初始的请求已经接受，请客户端继续发送剩余部分                 |
| 101    | 切换协议       | 客户要求服务器根据请求转换HTTP协议版本，服务器已确定切换，响应客户端的 Upgrade 标头发送的 |
|        |                |                                                              |
| 200    | 成功           | 服务器已成功处理了请求                                       |
| 201    | 已创建         | 请求成功并且服务器创建了新的资源                             |
| 202    | 已接受         | 服务器已接受请求，但尚未处理                                 |
| 203    | 非授权信息     | 服务器已成功处理请求，但返回的信息可能来自另一个来源         |
| 204    | 无内容         | 服务器成功处理了请求，但没有返回任何内容，没有响应体         |
| 205    | 重置内容       | 服务器处理成功，用户终端应重置文档视图                       |
| 206    | 部分内容       | 断点续传，服务器成功处理了部分GET请求，响应报文中包含由 Content-Range 指定范围的实体内容 |
|        |                |                                                              |
| 300    | 多种选择       | 针对请求，服务器可执行多种操作                               |
| 301    | 永久移动       | 请求的页面已永久跳转到新的url，域名需要切换、协议从http变成https；搜索引擎地址会变，SEO友好 |
| 302    | 临时移动       | 服务器目前从不同位置的网页响应请求，但请求仍继续使用原有位置来进行以后的请求，登录页面跳转；搜索引擎地址不变，可能出现网站劫持 |
| 303    | 查看其他位置   | 请求者应当对不同的位置使用单独的GET请求来检索响应时，服务器返回此代码 |
| 304    | 未修改         | 自从上次请求后，请求的网页未修改过                           |
| 307    | 临时重定向     | HSTS、服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。 |
| 308    | 永久重定向     | 资源现在永久位于由 `Location:`指定的另一个 URI，但用户代理不能更改所使用的 HTTP 方法 |
|        |                |                                                              |
| 400    | 错误请求       | 服务器不理解请求的语法                                       |
| 401    | 未授权         | 请求要求用户的身份演验证                                     |
| 403    | 禁止           | 服务器拒绝请求                                               |
| 404    | 未找到         | 服务器找不到请求的页面                                       |
| 405    | 方法禁用       | 禁用请求中指定的方法                                         |
| 406    | 不接受         | 无法使用请求的内容特性响应请求的页面                         |
| 407    | 需要代理授权   | 请求需要代理的身份认证                                       |
| 408    | 请求超时       | 服务器等候请求时发生超时                                     |
| 409    | 冲突           | 服务器在完成请求时发生冲突                                   |
| 410    | 已删除         | 客户端请求的资源已经不存在                                   |
| 411    | 需要有效长度   | 服务器不接受不含有效长度表头字段的请求                       |
| 412    | 未满足前提条件 | 服务器未满足请求者在请求中设置的其中一个前提条件             |
| 413    | 请求实体过大   | 由于请求实体过大，服务器无法处理，因此拒绝请求               |
| 414    | 请求url过长    | 请求的url过长，服务器无法处理                                |
| 415    | 不支持格式     | 服务器无法处理请求中附带媒体格式                             |
| 416    | 范围无效       | 请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求 |
| 417    | 未满足期望值   | 服务器未满足"期望"请求标头字段的要求                         |
|        |                |                                                              |
| 500    | 服务器错误     | 服务器内部错误，无法完成请求                                 |
| 501    | 尚未实施       | 服务器不具备完成请求的功能或不支持请求的函数                 |
| 502    | 错误网关       | 服务器作为网关或代理出现错误                                 |
| 503    | 服务不可用     | 服务器目前无法使用                                           |
| 504    | 网关超时       | 指服务器作为网关或代理，但是没有及时从上游服务器收到请求     |
| 505    | 协议版本无效   | HTTP版本不受支持                                             |



**重定向都有哪些状态码，区别是？**

有301、302、303、307、308

301：永久移动

302：临时移动

303：查看其他位置

307：临时重定向

308：永久重定向





## 常见HTTP头

HTTP头包括：通用信息头（请求头、响应头）、实体头

**通用信息头**：它取决于应用的上下文环境，可以是**响应头部**或者**请求头部**，但是**不能应用于消息内容自身**的**实体头部**。
	**响应头部**：它可在 HTTP **响应**中使用，并且**和请求主体无关** 。
	**请求头部**：它可在 HTTP **请求**中使用，并且**和请求主体无关** 。

**实体报头**：描述了一个 HTTP 消息**有效载荷**（即关于**消息主体的元数据**）的 HTTP 报头



### 请求头

- **内容协商**：
  - **Accept**：可接受类型
  - **Accept-Charset**：可接受字符集
  - **Accept-Encoding**：可接受编码
  - **Accept-Language**：可接受语言
  - **Accept-Datetime**：可接受的时间版本
- **跨域相关**：
  - **Origin**：表明预检请求或实际请求的源站。
  - **Access-Control-Request-Method**：首部字段用于预检请求，将实际请求所使用的 HTTP 方法告诉服务器。
  - **Access-Control-Request-Headers**：首部字段用于预检请求，将实际请求所携带的首部字段告诉服务器。

- **缓存相关**：
  - **Cache-Control**：缓存相关
  - **If-None-Match**：判断Etag
  - **If-Modified-Since**：判断Last-Modified

- **权限和安全相关**：
  - **Authorization**：包含用服务器验证用户代理的凭证
  - **Cookie**：凭证储存
  - **X-Csrf-Token**：用于防止跨站请求伪造

- **连接相关**：
  - **Connection**：keep-alive持久连接、close断开连接
  - **Host**：指定被请求资源的Internet主机和端口号
  - **Referer**：告诉服务器我是从哪个页面链接过来的
  - **User-Agent**：客户端的操作系统和浏览器
  - **Range**：断点续传，告诉服务器仅请求某个实体的一部分



### 响应头

- **内容协商相关**
  - **Content-Type**：MIME类型
  - **Content-Encoding**：编码
  - **Content-Language**：语言
  - **Content-Length**：内容长度
  - **Content-Location**：资源所在位置
  - **Content-Range**：断点传输，数据内容的起止位 置以及整个需要请求的内容的长度
- **跨域相关**
  - **Access-Control-Allow-Origin**：跨域白名单
  - **Access-Control-Allow-Credentials**：跨域是否允许cookies
  - **Access-Control-Allow-Methods**：预检请求响应，指明了实际请求中允许使用的方法。
  - **Access-Control-Allow-Headers**：预检请求响应，指明了实际请求中允许携带的首部字段。
  - **Access-Control-Max-Age**：预检请求的返回结果缓存时间
  - **Access-Control-Expose-Headers**：在跨源访问时，XHR能获取的请求头白名单。
- **缓存相关**
  - **Cache-Control**：缓存
  - **Expires**：缓存到期时间
  - **Etag**：资源标识
  - **Last-Modified**：最后修改时间
  - **Age**：这个对象在代理缓存中存在的时间，以秒为单位
- **安全相关**
  - **Content-Security-Policy**：CSP，网页安全政策，白名单制度，防止 XSS 攻击
  - **X-XSS-Protection**：0: 禁止XSS过滤、1 启用 XSS 过滤（通常在浏览器中默认）、1; mode=block 启用XSS保护，并在检查到XSS攻击时，停止渲染页面
  - **X-FRAME-OPTIONS**：DENY，拒绝被嵌入IFrame；SAMEORIGIN：只能被同域网站嵌套或加载；ALLOW-FROM URL：可以被指定网站嵌套
  - **X-Content-Type-Options**：nosniff，过滤掉不安全的文件(只允许对应标签加载对应MIME)
  - **Strict-Transport-Security**：HSTS安全策略
  - **Set-Cookie**：设置和页面关联的Cookie
- **连接相关**：
  - **Allow**：当遇到405状态码时，服务器给出允许的方法
  - **Connection**：keep-alive持久连接、close断开连接
  - **Upgrade**：要求客户端升级到另一个协议，例如 websocket
  - **Transfer-Encoding**：chunked，分块发送；gzip，压缩等
- **跳转相关**：
  - **Refresh**：重定向
  - **Location**：表示客户应当到哪里去提取文档。
  - **Refresh**：浏览器应该在多少时间之后刷新文档，以秒计。
- **信息**
  - **Date**：响应发送时间
  - **Server**：服务器版本



### Content-Type MIME类型：

1. **application/x-www-form-urlencoded**：浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据。该种方式提交的数据放在 body 里面，数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL转码。
2. **multipart/form-data**：该种方式也是一个常见的 POST 提交方式，通常表单上传文件时使用该种方式。
3. **application/json**：服务器消息主体是序列化后的 JSON 字符串。
4. **text/xml**：该种方式主要用来提交 XML 格式的数据。



## 请求方法

| 请求方法 | 作用                                                  | HTTP版本 | 是否有请求体 | 是否返回响应体 | 安全性(不修改服务器资源) | 幂等 | 缓存性                                    | 表单方式 | 相关状态码    |
| -------- | ----------------------------------------------------- | -------- | ------------ | -------------- | ------------------------ | ---- | ----------------------------------------- | -------- | ------------- |
| Get      | 获取资源                                              | 0.9      | ×            | √              | √                        | √    | √                                         | √        |               |
| Post     | 将实体提交到指定的资源                                | 1.0      | √            | √              | ×                        | ×    | Only if freshness information is included | √        |               |
| Put      | 替换更新资源                                          | 1.0      | √            | ×              | ×                        | √    | ×                                         | ×        | 200、201、204 |
| Delete   | 删除某一个资源                                        | 1.0      | √            | √              | ×                        | √    | ×                                         | ×        | 200、202、204 |
| Head     | 获取报文首部、下载前获取文件大小                      | 1.0      | ×            | ×              | √                        | √    | √                                         | ×        |               |
| Options  | 预检请求、获取可用的方法、了解服务器的性能            | 1.1      | ×            | √              | √                        | √    | ×                                         | ×        |               |
| Trace    | 回显服务器收到的请求，主要⽤于测试或诊断              | 1.1      | ×            | ×              | √                        | √    | ×                                         | ×        | 200           |
| Connect  | 要求在与代理服务器通信时建立隧道，使用隧道进行TCP通信 | 1.1      | ×            | √              | ×                        | ×    | ×                                         | ×        |               |
| Patch    | Put方法的补充，用于局部修改的资源                     | 1.1      | √            | √              | ×                        | ×    | ×                                         | ×        | 204           |

幂等：同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的。



## GET方法URL长度限制的原因

实际上HTTP协议规范并没有对get方法请求的url长度进行限制，这个限制是特定的浏览器及服务器对它的限制。 IE对URL长度的限制是2083字节(2K+35)。由于IE浏览器对URL长度的允许值是最小的，所以开发过程中，只要URL不超过2083字节，那么在所有浏览器中工作都不会有问题。

下面看一下主流浏览器对get方法中url的长度限制范围：

- Microsoft Internet Explorer (Browser)：IE浏览器对URL的最大限制为2083个字符，如果超过这个数字，提交按钮没有任何反应。
- Firefox (Browser)：对于Firefox浏览器URL的长度限制为 65,536 个字符。
- Safari (Browser)：URL最大长度限制为 80,000 个字符。
- Opera (Browser)：URL最大长度限制为 190,000 个字符。
- Google (chrome)：URL最大长度限制为 8182 个字符。

主流的服务器对get方法中url的长度限制范围：

- Apache (Server)：能接受最大url长度为8192个字符。
- Microsoft Internet Information Server(IIS)：能接受最大url的长度为16384个字符。

根据上面的数据，可以知道，get方法中的URL长度最长不超过2083个字符，这样所有的浏览器和服务器都可能正常工作。



## HTTP2的头部压缩算法是怎样的？

HTTP2的头部压缩是**HPACK算法**。在客户端和服务器两端建立“**字典**”，用索引号表示重复的字符串，采用**哈夫曼编码**来压缩整数和字符串，可以达到50%~90%的高压缩率。

- 在客户端和服务器端使用“**首部表**”来跟踪和存储之前发送的键值对，对于相同的数据，不再通过每次请求和响应发送；
- 首部表在HTTP/2的连接存续期内始终存在，由客户端和服务器共同**渐进**地更新；
- 每个新的首部键值对要么被**追加**到当前表的**末尾**，要么**替换**表中之前的值。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bda10d6c69e74996b6505777d29b9a74~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



## 说一下HTTP 3.0

HTTP/3基于UDP协议实现了类似于TCP的多路复用数据流、传输可靠性等功能，这套功能被称为QUIC协议。 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45a0a2ec0ef143b49d79256cea543418~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

1. 流量控制、传输可靠性功能：QUIC在UDP的基础上增加了一层来保证数据传输可靠性，它提供了数据包重传、拥塞控制、以及其他一些TCP中的特性。
2. 集成TLS加密功能：目前QUIC使用TLS1.3，减少了握手所花费的RTT数。
3. 多路复用：同一物理连接上可以有多个独立的逻辑数据流，实现了数据流的单独传输，解决了TCP的队头阻塞问题。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48df233ce8e541efa165160c169b7a70~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

1. 快速握手：由于基于UDP，可以实现使用0 ~ 1个RTT来建立连接。



## HTTP协议的优点和缺点

HTTP 是超文本传输协议，它定义了客户端和服务器之间交换报文的格式和方式，默认使用 80 端口。它使用 TCP 作为传输层协议，保证了数据传输的可靠性。

HTTP协议具有以下**优点**：

- 支持**客户端/服务器模式**
- **简单快速**：客户向服务器请求服务时，只需传送请求方法和路径。由于 HTTP 协议简单，使得 HTTP 服务器的程序规模小，因而通信速度很快。
- **无连接**：无连接就是**限制每次连接只处理一个请求**。服务器处理完客户的请求，并收到客户的应答后，即断开连接，采用这种方式可以**节省传输时间**。
- **无状态**：HTTP 协议是无状态协议，这里的状态是指**通信过程的上下文信息**，在服务器不需要先前信息时它的应答就比较快（优），但如果后续处理需要前面的信息，则它必须重传，这样可能会导致每次连接传送的数据量增大（缺）。
- **灵活**：HTTP 允许传输**任意类型**的数据对象。正在传输的类型由 Content-Type 加以标记。

HTTP协议具有以下**缺点**：

- **无状态：**HTTP **服务器不会保存关于客户的任何信息**。
- **明文传输：** 协议中的报文使用的是文本形式，这就直接暴露给外界，不安全。
- **不安全**：通信使用**明文**，内容可能会被**窃听**；**不验证**通信方的**身份**，因此有可能遭遇**伪装**；无法证明报文的**完整性**，所以有可能已遭**篡改**；



## HTTP协议的性能怎么样（长连接）

HTTP 协议是基于 TCP/IP，并且使用了**请求-应答**的通信模式，所以性能的关键就在这两点里。HTTP协议有两种连接模式，一种是持续连接，一种非持续连接。

- **非持续连接**：指的是服务器必须为每一个请求的对象建立和维护一个全新的连接。
- **持续连接**：TCP 连接默认不关闭，可以被多个请求复用。采用持续连接的好处是可以避免每次建立 TCP 连接三次握手时所花费的时间。

- **HTTP/1.0**：默认采用非持续连接，每发起一个请求，都要新建一次 TCP 连接（三次握手），而且是串行请求，做了无畏的 TCP 连接建立和断开，增加了通信开销。该版本使用的非持续的连接，但是可以在请求时，加上 Connection: keep-alive 来要求服务器不要关闭 TCP 连接。
- **HTTP/1.1**：默认采用非持续连接(长连接)。这种方式的好处在于减少了 TCP 连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。该版本及以后版本默认采用的是持续的连接。目前对于同一个域，大多数浏览器支持同时建立 6 个持久连接。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f756e23ecf3a4d2a80d632b5fa8b0b6d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

- **管道网络传输**：HTTP/1.1 采用了长连接的方式，这使得管道（pipeline）网络传输成为了可能。管道（pipeline）网络传输是指：**可以在同一个 TCP 连接里面，客户端可以发起多个请求**，只要第一个请求发出去了，不必等其回来，就可以发第二个请求出去，可以减少整体的响应时间。但是服务器还是按照顺序回应请求。如果前面的回应特别慢，后面就会有许多请求排队等着。这称为队头堵塞。

- **队头堵塞**：HTTP 传输的报文必须是一发一收，但是，里面的任务被放在一个任务队列中**串行**执行，一旦队首的请求处理太慢，就会阻塞后面请求的处理。这就是HTTP队头阻塞问题。队头阻塞的**解决方案**：
  - **并发**连接：对于一个域名允许分配多个长连接，那么相当于增加了任务队列，不至于一个队伍的任务阻塞其它所有任务。
  - **域名分片**：将域名分出很多**二级域名**，它们都指向同样的一台服务器，能够并发的长连接数变多，解决了队头阻塞的问题。



## 对keep-alive的理解

HTTP1.0 中默认是在每次请求/应答，客户端和服务器都要新建一个连接，完成之后立即断开连接，这就是**短连接**。当使用Keep-Alive模式时，Keep-Alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接，这就是**长连接**。

**HTTP1.0**：是默认没有Keep-alive的（也就是默认会发送keep-alive），所以要想连接得到保持，必须手动配置发送`Connection: keep-alive`字段。若想断开keep-alive连接，需发送`Connection:close`字段；
**HTTP1.1**：默认保持长连接，数据传输完成了保持TCP连接不断开，等待在同域名下继续用这个通道传输数据。如果需要关闭，需要客户端发送`Connection：close`首部字段。

Keep-Alive的**建立过程**：

- 客户端向服务器在发送请求报文同时在首部添加发送**Connection字段**
- 服务器收到请求并处理 Connection字段
- 服务器回送Connection:Keep-Alive字段给客户端
- 客户端接收到Connection字段
- Keep-Alive连接建立成功

**服务端自动断开过程（也就是没有keep-alive）**：

- 客户端向服务器只是发送内容报文（不包含Connection字段）
- 服务器收到请求并处理
- 服务器返回客户端请求的资源并关闭连接
- 客户端接收资源，发现没有Connection字段，断开连接

**客户端请求断开连接过程**：

- 客户端向服务器发送Connection:close字段
- 服务器收到请求并处理connection字段
- 服务器回送响应资源并断开连接
- 客户端接收资源并断开连接

**优点**：

- 较少的CPU和内存的使⽤（由于同时打开的连接的减少了）；
- 允许请求和应答的HTTP管线化；
- 降低拥塞控制 （TCP连接减少了）；
- 减少了后续请求的延迟（⽆需再进⾏握⼿）；
- 报告错误⽆需关闭TCP连；

**缺点**：

- 长时间的Tcp连接容易导致系统资源无效占用，浪费系统资源。



##  页面有多张图片，HTTP是怎样的加载表现？

- 在`HTTP 1`下，浏览器对**一个域名下最大TCP连接数为6**，所以会请求多次。可以用**多域名部署**解决。这样可以提高同时请求的数目，加快页面图片的获取速度。
- 在`HTTP 2`下，可以一瞬间加载出来很多资源，因为，HTTP2支持**多路复用**，可以在一个TCP连接中发送多个HTTP请求。



## URL有哪些组成部分

- **协议** :// **域名** : **端口** / **虚拟目录** / **文件名** # **锚** ? **参数 : 值**

