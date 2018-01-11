<!-- TOC -->

- [HTTP 报文](#http-报文)
    - [报文与实体](#报文与实体)
    - [报文结构](#报文结构)
    - [报文的分类](#报文的分类)
    - [HTTP 报文首部字段](#http-报文首部字段)
        - [通用首部字段(General Header Fields)](#通用首部字段general-header-fields)
        - [请求首部字段(Request Header Fields)](#请求首部字段request-header-fields)
        - [响应首部字段(Response Header Fields)](#响应首部字段response-header-fields)
        - [实体首部字段(Entity Header Fields)](#实体首部字段entity-header-fields)
    - [使用编码](#使用编码)
        - [内容编码](#内容编码)
        - [分块传输编码（Chunked Transfer Coding)](#分块传输编码chunked-transfer-coding)
    - [多部分对象集合](#多部分对象集合)
    - [内容协商](#内容协商)

<!-- /TOC -->

# HTTP 报文

> 用于 HTTP 协议交互的信息被称为 HTTP 报文。它是由多行(用 CR+LF 作换行符)数据构成的字符串文本。

请求端(客户端)的 HTTP 报文叫做请求报文，响应端(服务器端)的叫做响应报文。

## 报文与实体

报文是箱子，实体是货物。

* 报文(message):

  是 HTTP 通信中的基本单位，由 8 位组字节流(octet sequence，其中 octet 为 8 个比特)组成，通过 HTTP 通信传输。

* 实体(entity):

  作为请求或响应的有效载荷数据(补充项)被传输，其内容由实体首部和实体主体组成。

通常，报文主体等于实体主体。只有当传输中进行编码操作时，实体主体的内容发生变化，才导致它和报文主体产生差异。

## 报文结构

HTTP 报文大致可分为报文首部和报文主体两块。两者由空行(CR+LF)来划分。其中，报文主体是非必需的。

报文结构如下

```
报文首部
空行(CR+LF)
报文主体
```

<!-- 首部内容：

请求行／状态行

首部字段（通用首部、请求首部、响应首部和实体首部。）

其他（Cookie 等） -->

## 报文的分类

HTTP 报文主要分为俩类：

* 请求报文

  由方法、URI、HTTP 版本、HTTP 首部字段等部分构成。

* 响应报文

  由 HTTP 版本、状态码(数字和原因短语)、HTTP 首部字段 3 部分构成。

## HTTP 报文首部字段

> HTTP 首部字段是构成 HTTP 报文的要素之一。在客户端与服务器之间以 HTTP 协议进行通信的过程中，无 论是请求还是响应都会使用首部字段，它能起到传递额外重要信息的作用。

HTTP 首部字段是由首部字段名和字段值构成的，中间用冒号“:” 分隔。

```
首部字段名: 字段值
```

**注意：** 针对报文首部中出现重复字段的情况，各浏览器内部处理逻辑会有所不同，结果可能并不一致。有些浏览器会优先处理第一次出现的首部字 段，而有些则会优先处理最后出现的首部字段。

### 通用首部字段(General Header Fields)

| Header              | 解释                                                                                           | 示例                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Accept              | 指定客户端能够接收的内容类型                                                                   | Accept: text/plain, text/html                           |
| Accept-Charset      | 浏览器可以接受的字符编码集。                                                                   | Accept-Charset: iso-8859-5                              |
| Accept-Encoding     | 指定浏览器可以支持的 web 服务器返回内容压缩编码类型。                                          | Accept-Encoding: compress, gzip                         |
| Accept-Language     | 浏览器可接受的语言                                                                             | Accept-Language: en,zh                                  |
| Accept-Ranges       | 可以请求网页实体的一个或者多个子范围字段                                                       | Accept-Ranges: bytes                                    |
| Authorization       | HTTP 授权的授权证书                                                                            | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==       |
| Cache-Control       | 指定请求和响应遵循的缓存机制                                                                   | Cache-Control: no-cache                                 |
| Connection          | 表示是否需要持久连接。（HTTP 1.1 默认进行持久连接）                                            | Connection: close                                       |
| Cookie              | HTTP 请求发送时，会把保存在该请求域名下的所有 cookie 值一起发送给 web 服务器。                 | Cookie: $Version=1; Skin=new;                           |
| Content-Length      | 请求的内容长度                                                                                 | Content-Length: 348                                     |
| Content-Type        | 请求的与实体对应的 MIME 信息                                                                   | Content-Type: application/x-www-form-urlencoded         |
| Date                | 请求发送的日期和时间                                                                           | Date: Tue, 15 Nov 2010 08:12:31 GMT                     |
| Expect              | 请求的特定的服务器行为                                                                         | Expect: 100-continue                                    |
| From                | 发出请求的用户的 Email                                                                         | From: user@email.com                                    |
| Host                | 指定请求的服务器的域名和端口号                                                                 | Host: www.zcmhi.com                                     |
| If-Match            | 只有请求内容与实体相匹配才有效                                                                 | If-Match: “737060cd8c284d8af7ad3082f209582d”            |
| If-Modified-Since   | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回 304 代码                          | If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT        |
| If-None-Match       | 如果内容未改变返回 304 代码，参数为服务器先前发送的 Etag，与服务器回应的 Etag 比较判断是否改变 | If-None-Match: “737060cd8c284d8af7ad3082f209582d”       |
| If-Range            | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为 Etag                    | If-Range: “737060cd8c284d8af7ad3082f209582d”            |
| If-Unmodified-Since | 只在实体在指定时间之后未被修改才请求成功                                                       | If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT      |
| Max-Forwards        | 限制信息通过代理和网关传送的时间                                                               | Max-Forwards: 10                                        |
| Pragma              | 用来包含实现特定的指令                                                                         | Pragma: no-cache                                        |
| Proxy-Authorization | 连接到代理的授权证书                                                                           | Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== |
| Range               | 只请求实体的一部分，指定范围                                                                   | Range: bytes=500-999                                    |
| Referer             | 先前网页的地址，当前请求网页紧随其后,即来路                                                    | Referer: http://www.zcmhi.com/archives/71.html          |
| TE                  | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息                                       | TE: trailers,deflate;q=0.5                              |
| Upgrade             | 向服务器指定某种传输协议以便服务器进行转换（如果支持）                                         | Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11          |
| User-Agent          | User-Agent 的内容包含发出请求的用户信息                                                        | User-Agent: Mozilla/5.0 (Linux; X11)                    |
| Via                 | 通知中间网关或代理服务器地址，通信协议                                                         | Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)             |
| Warning             | 关于消息实体的警告信息                                                                         | Warn: 199 Miscellaneous warning                         |

### 请求首部字段(Request Header Fields)
| Header              | 解释                                                                                           | 示例                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Accept              | 指定客户端能够接收的内容类型                                                                   | Accept: text/plain, text/html                           |
| Accept-Charset      | 浏览器可以接受的字符编码集。                                                                   | Accept-Charset: iso-8859-5                              |
| Accept-Encoding     | 指定浏览器可以支持的 web 服务器返回内容压缩编码类型。                                          | Accept-Encoding: compress, gzip                         |
### 响应首部字段(Response Header Fields)
| Header              | 解释                                                                                           | 示例                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Accept              | 指定客户端能够接收的内容类型                                                                   | Accept: text/plain, text/html                           |
| Accept-Charset      | 浏览器可以接受的字符编码集。                                                                   | Accept-Charset: iso-8859-5                              |
| Accept-Encoding     | 指定浏览器可以支持的 web 服务器返回内容压缩编码类型。                                          | Accept-Encoding: compress, gzip                         |

### 实体首部字段(Entity Header Fields)
| Header              | 解释                                                                                           | 示例                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Accept              | 指定客户端能够接收的内容类型                                                                   | Accept: text/plain, text/html                           |
| Accept-Charset      | 浏览器可以接受的字符编码集。                                                                   | Accept-Charset: iso-8859-5                              |
| Accept-Encoding     | 指定浏览器可以支持的 web 服务器返回内容压缩编码类型。                                          | Accept-Encoding: compress, gzip                         |

## 使用编码

* 通过在传输时编码，能有效地处理大量的访问请求，提升传输速率
* 会消耗更多的 CPU 等 资源。

实现方式及原理：

* 服务端根据指定方式对实体内容进行编码，传输到客户端
* 客户端再编码规则对实体内容进行解码

### 内容编码

内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。

常用的内容编码：

* gzip(GNU zip)
* compress(UNIX 系统的标准压缩)
* deflate(zlib)
* identity(不进行编码)

### 分块传输编码（Chunked Transfer Coding)

把实体主体分块的功能称为分块传输编码。每一块都会用十六进制来标记块的大小，而实体主体的最后一块会使用“0(CR+LF)”来标记。

## 多部分对象集合

允许报文主体内可含有多类型实体。

使用时需要在首部字段里加上 Content-type。

* multipart/form-data

  在 Web 表单文件上传时使用。

* multipart/byteranges

  可以指定资源的字节范围，响应报文包含了多个范围的内容时使用， 态码 206(Partial Content，部分内容)。

## 内容协商

> 内容协商机制是指客户端和服务器端就响应的资源内容进行交涉，然后提供给客户端最为适合的资源。内容协商会以响应资源的语言、字符集、编码方式等作为判断的基准。

这些基准可以在报文中通过首部字段指定：

`Accept/Accept-Charset/Accept-Encoding/Accept-Language/Content-Language`

1. 服务器驱动协商

   服务器端以请求的首部字段为参考，在服务器端自动处理

2. 客户端驱动协商

   用户从客户端显示的可选项列表中手动选择。

3. 透明协商

   服务器驱动和客户端驱动的结合体
