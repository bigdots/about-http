<!-- TOC -->

- [http状态码](#http状态码)
    - [类别](#类别)
        - [1XX](#1xx)
        - [2XX](#2xx)
        - [3XX](#3xx)
        - [4XX](#4xx)
        - [5XX](#5xx)

<!-- /TOC -->

# http状态码

HTTP 状态码负责表示客户端 HTTP 请求的返回结果、标记服务器端的处理结果。

状态码以 3 位数字和原因短语组成。 数字中的第一位指定了响应类别，后两位无分类


## 类别

### 1XX

Informational(信息性状态码)
接收的请求正在处理

### 2XX

Success(成功状态码)
请求正常处理完毕

1. 200 OK

    请求成功。
2. 204 No Content

    请求成功。返回的响应报文中不含实体的主体部分
3. 206 Partial Content

    范围请求成功

### 3XX

Redirection(重定向状态码)
需要进行附加操作以完成请求

1. 301 Moved Permanently

    永久性重定向。如果已经把资源对应的 URI 保存为书签了，这时应该按 Location 首部字段提示的 URI 更新书签。

2. 302 Found

    临时性重定向。只在(本次)使用新的 URI 访问。

3. 303 See Other

    303 和 302 有着相同的功能，但 303 状态码明确表示客户端应当采用 GET 方法获取资
源。

4. 304 Not Modified

    表示服务器端允许请求访问资源，但未满足条件的情况，比如缓存没到期。**注意：** 304 虽然被划分在 3XX 类别中，但是和重定向没有关系。

    通常需要根` If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since`中任一首部配合使用。

5. 307 Temporary Redirect

    临时重定向。该状态码与 302 有着相同的含义。但是它会遵照浏览器标准，不会从 POST 变成 GET。

### 4XX

Client Error(客户端错误状态码)
服务器无法处理请求

1. 400 Bad Request

    表示请求报文中存在语法错误。浏览器会像 200 OK 一样对待该状态码。

2. 401 Unauthorized

    表示发送的请求需要有通过 HTTP 认证（BASIC/DIGEST）的认证信息。

3. 403 Forbidden

    表明对请求资源的访问被服务器拒绝了。未获得文件系统的访问授权，访问权限出现某些问题等情况都可能是发生 403 的原因。

4. 404 Not Found

    表明服务器上无法找到请求的资源。

### 5XX

Server Error(服务器错误状态码)
服务器处理请求出错

1. 500 Internal Server Error

    表明服务器端在执行请求时发生了错误。

2. 503 Service Unavailable

    表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。