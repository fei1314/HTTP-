http:
一、无状态的协议
二、客户端和服务端的连接过程：三次握手
三、客户端和服务端的数据交互
  1.客户端向服务端发送一个HTTP请求消息Request
    GET /web/20171110/data/2.txt HTTP/1.1
    Host: localhost
    Connection: keep-alive
    Pragma: no-cache
    Cache-Control: no-cache
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36
    Accept: */*
    Referer: http://localhost/web/20171110/XMLHttpRequest6.html
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en,zh-CN;q=0.8,zh;q=0.6

    请求消息包括请求行、请求头部、空行、请求数据四个部分。

    （1）请求行：请求方式，请求的URL，HTTP协议版本 GET /web/20171110/data/2.txt HTTP/1.1
    （2）请求头部：用来说明服务器使用的附加信息
        Host：请求的目的地
        Connection：http的连接状态
        User-Agent:服务器端和客户端脚本都能访问它,它是浏览器类型检测逻辑的重要基础.该信息由你的浏览器来定义,并且在每个请求中自动发送
        Referer：用来让服务器判断来源页面, 即用户是从哪个页面来的,通常被网站用来统计用户来源,是从搜索页面来的,还是从其他网站链接过来,或是从书签等访问,以便网站合理定位.
        Accept-Encoding：浏览器发给服务器,声明浏览器支持的编码类型
    （3）空行：请求头部的后面必须是空行。即使没有请求数据，也有空行。
    （4）请求数据：请求数据也叫主体，可以添加任意的其他数据。

    2.服务端接收并处理客户端发送过来的请求后返回一个HTTP的响应消息Response
      HTTP/1.1 200 OK
      Date: Sat, 11 Nov 2017 12:28:48 GMT
      Server: Apache/2.4.18 (Win32) OpenSSL/1.0.2e PHP/5.6.17
      Last-Modified: Sat, 11 Nov 2017 11:51:48 GMT
      ETag: "1f-55db3a98b5cdc"
      Accept-Ranges: bytes
      Content-Length: 31
      Keep-Alive: timeout=5, max=99
      Connection: Keep-Alive
      Content-Type: text/plain

      响应消息包括状态行、消息报头、空行、响应正文四个部分。

      （1）状态行：HTTP协议版本、状态码、状态消息
      （2）消息报头：客户端使用的一些附加信息
          Date:日期
          Content-Type:指定了MIME类型
      （3）空行：空行是必须有的
      （4）响应正文：服务端返回给客户端的文本信息
四、HTTP状态码（status）
  1xx:指示信息--表示请求已接收，继续处理
  2xx:成功--表示请求已经被接收、理解、接受
  3xx：重定向--要完成请求必须进一步操作
  4xx:客户端错误--请求有语法错误或请求无法实现
  5xx:服务端错误--服务端不能完成合法的请求

  常见的状态码：
    100   Continue                //服务端已经接受初始的请求，客户端应继续发送其余的请求
    101   Switching Protocols     //服务端将遵从客户端转到另一种协议
    200   OK                      //客户端请求成功
    201   Created                 //服务端已经创建了文档，Location头给出了它的URL
    202   Accepted                //已经接受请求，还没有出来完成
    203  Non-Authoritative Information  //文档正常地返回，但一些应答头不正确，因为使用的是文档的拷贝
    205 Reset Content   //没有新的内容，但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容
    206 Partial Content //客户发送了一个带有Range头的GET请求，服务器完成了它
    301   Moved Permanently       //永久重定向，下回不会再找他了
    302   Move temporarily        //临时重定向，下回依然会请求服务器
    304   Not Modified            //缓存
    305   Use Proxy               //客户请求的文档应该通过Location头所指明的代理服务器提取
    400   Bad Request             //客户端请求出现语法错误
    403   Forbidden               //服务器收到请求，但是拒绝提供服务
    404   Not Found               //请求资源不存在
    405   Method Not Allowed      //请求方法（GET、POST、HEAD、Delete、PUT等）对指定的资源不适用。
    409   Conflict                //由于请求和资源的当前状态相冲突，因此请求不能成功。
    500   Internal Server Error   //服务器发生不可预期的错误
    503   Service Unavailable     //服务器由于维护或者负载过重未能应答
    505   HTTP Version Not Supported  //服务器不支持请求中所指明的HTTP版本
    更多状态码http://www.runoob.com/http/http-status-codes.html
五、readyState
    0   初始状态      xhr对象刚创建完
    1   连接          连接到服务器
    2   发送请求      刚刚send完
    3   接收完成      头接收完了
    4   接收完成      体接收完了
六、HTTP请求方法
  GET/POST/PUT/DELETE/HEAD

  GET     把数据放在url里面传输        数据量很小、缓存、看得见        <=32K
  POST    放在body里                  数据量大、不会缓存、看不见      <=1G

  GET——获取东西
  POST、PUT——发送东西      大量发送
  DELETE——删除
  HEAD——让服务器只发送头回来就行(不需要内容)
七、HTTP工作原理
  1、客户端连接到服务端
    客户端与服务端建立TCP链接
  2、客户端向服务端发送HTTP请求
    通过TCP链接，客户端向服务端发送一个文本的请求报文，请求报文包括请求行、请求头部、空行、请求数据
  3、服务端接收请求并返回HTTP响应
    服务端解析请求，定位请求资源，服务端将资源复制到TCP套接字上，将响应返回给客户端。一个响应包括状态行、响应头部、空行、响应正文。
  4、释放TCP链接
    若connection 模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;
  5、客户端解析返回过来的HTML内容
    客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。
八、在浏览器输入URL，到显示页面都发生了什么？
  1、浏览器向DNS服务器请求解析URL中域名对应的IP地址
  2、建立TCP链接
  3、发送HTTP请求
  4、服务端对浏览器的请求做出响应，将html返回给浏览器
  5、释放TCP链接
  6、浏览器解析HTML内容显示页面
