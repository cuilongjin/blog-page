
## HTTP协议

**协议：**

> 协议，就是事先的一种约定、规则、规范、标准

 **常见协议**

- HTTP、HTTPS 超文本传输协议
- FTP 文件传输协议
- SMTP 简单邮件传输协议

**HTTP 协议**

HTTP 协议即超文本传输协议,  是一个 [浏览器端] 和 [服务器端] 请求和响应的标准

- 常用请求方法  GET, POST
- 请求 (request)：`请求行、请求头、请求主体`。
- 响应 (response)：`状态行、响应头、响应主体`。



### 请求和请求报文

​	请求由浏览器发起，其规范格式为：请求行、请求头、请求主体。

**get 请求的请求报文**

```http
// ---------请求行---------
// GET 请求方式
// /day02/01.php?username=pp&password=123456    请求路径+参数（注意点）
// HTTP/1.1 HTTP的版本号
GET /day03/01.php?username=pp&password=123456 HTTP/1.1

//----------请求头---------
// Host:主机地址
Host: www.study.com
// HTTP1.1版本默认开启，建立过连接后，TCP连接不会断开，下次连接可以继续使用（底层，不用管）
Connection: keep-alive
// chrome浏览器自己增加的，不用管
Upgrade-Insecure-Requests: 1
// 浏览器的代理字符串（版本信息）
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.96 Safari/537.36
// 浏览器端可以接受的类型。
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,`*/*`;q=0.8
// 从哪个页面发出的请求
Referer: http: // www.study.com/day02/01-login.html
// 检查浏览器支持的压缩方式
Accept-Encoding: gzip, deflate, sdch
// 浏览器支持的语言，优先中文。
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6

// -----------请求主体-----------
// get 请求没有请求体，因为要传递的数据已经拼接到了请求主头中
```



**POST 请求的请求报文**

```http
// -----------------------请求行---------------------------------------------
POST /day02/01.php HTTP/1.1

// -----------------------请求头--------------------------------------------
Host: www.study.com
Connection: keep-alive
// 传递的参数的长度
Content-Length: 29
Cache-Control: max-age=0
Origin: http: // www.study.com
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.96 Safari/537.36
// 内容类型：表单数据，如果是post请求，必须指定这个属性。
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,`*/*`;q=0.8
Referer: http: // www.study.com/day02/01-login.html
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6

// ------------------------请求体------------------------------------------
username=pp&password=123456
```



**`GET` 请求与 `POST` 请求的对比**

- GET 请求没有请求体，因为 GET 请求的参数拼接到地址栏中了
- POST 请求有请求体，就是传递的参数。



### 响应与响应报文

​	响应由服务器发出，其规范格式为：响应行(状态行)、响应头、响应主体。

```http
// ---------------------响应行（状态行）-------------------------------
// HTTP/1.1  HTTP版本
// 200 响应的状态
	// 200表示成功
	// 302页面重定向
	// 304表示文档未修改
	// 404表示找不到资源
	// 500表示服务端错误
HTTP/1.1 200 OK

// ----------------------响应头-----------------------------------------------
Date: Thu, 22 Jun 2017 16:51:22 GMT // 服务器的时间
Server: Apache/2.4.23 (Win32) OpenSSL/1.0.2j PHP/5.4.45  // 服务器的版本信息
X-Powered-By: PHP/5.4.45  // 后台编程语言信息
Content-Length: 18   // 服务器的响应主体长度
// 内容类型，告诉浏览器该如何解析响应结果
Content-Type: text/html;charset=utf-8

// -----------------------响应主体------------------------------------------------
用户登录成功
```


常见状态码

2XX 成功

200 OK，表示从客户端发来的请求在服务器端被正确处理
204 No content，表示请求成功，但响应报文不含实体的主体部分
205 Reset Content，表示请求成功，但响应报文不含实体的主体部分，但是与 204 响应不同在于要求请求方重置内容
206 Partial Content，进行范围请求
3XX 重定向

301 moved permanently，永久性重定向，表示资源已被分配了新的 URL
302 found，临时性重定向，表示资源临时被分配了新的 URL
303 see other，表示资源存在着另一个 URL，应使用 GET 方法获取资源
304 not modified，表示服务器允许访问资源，但因发生请求未满足条件的情况
307 temporary redirect，临时重定向，和302含义类似，但是期望客户端保持请求方法不变向新的地址发出请求
4XX 客户端错误

400 bad request，请求报文存在语法错误
401 unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息
403 forbidden，表示对请求资源的访问被服务器拒绝
404 not found，表示在服务器上没有找到请求的资源
5XX 服务器错误

500 internal sever error，表示服务器端在执行请求时发生了错误
501 Not Implemented，表示服务器不支持当前请求所需要的某个功能
503 service unavailable，表明服务器暂时处于超负载或正在停机维护，无法处理请求
