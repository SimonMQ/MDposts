#### 各种概念
- TCP/IP协议 传输控制协议/因特网互联协议、网络通讯协议。定义设备如何连入因特网，以及数据怎样在它们之间传输。
- HTTP协议 超文本传输协议。用于浏览器和服务器之间的通信，传输HTML。
- HTML 超文本标记语言。它是一种标准，用来定义网页的文本(包括文字、图片、连接等)。
#### HTTP请求过程
- 浏览器向服务器发送HTTP请求，包括：
  - GET/POST GET仅请求资源，POST附带数据
  - 路径 /full/url/path
  - 域名 Host: www.sina.com.cn
  - 其它Header
  - 如果为POST，则还包括一个Body
- 服务器向浏览器返回HTTP响应，包括：
  - 响应代码 200成功、3xx重定向、4xx客户端请求错误、5xx服务器错误
  - 响应类型 由Content-Type指定
  - 其它Header
  - Body 响应的内容，包含网页的HTML源码
- 如果浏览器还需要继续请求资源，如图片，就再次重复1、2步骤。
Web采用的HTTP协议采用了简单的“请求-响应”模式，一个HTTP请求只处理一个资源。
#### HTTP格式
一个HTTP请求包含两部分：
- Header
- Body（可选）
##### HTTP GET请求格式：(每个Header一行，换行符\r\n)
> GET /path HTTP/1.1
Header1: Value1
Header2: Value2
Header3: Value3
##### HTTP POST请求格式：
> POST /path HTTP/1.1
Header1: Value1
Header2: Value2
Header3: Value3
body data goes here...
##### HTTP 响应格式：(\r\n\r\n来分割header与body)
> 200 OK
Header1: Value1
Header2: Value2
Header3: Value3
body data goes here...