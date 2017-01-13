## HttpClient 教程

该教程原文来自官方提供的 `httpcomponents-client-4.5.2` 包下 tutorial 目录的 pdf 教程。
如需要阅读原文，在 [apache 官网](http://hc.apache.org/downloads.cgi) 下载 HttpClient 压缩包解压后可以找到相关教程。

---

### 前言

Hyper-Text Transfer Protocol（HTTP 超文本传输协议）应该是应用在互联网上最重要的协议了。
网络服务，网络使能应用以及云计算的增多，在增加需要 HTTP 支持的应用数量时，不断的扩展 HTTP 协议在用户主导的浏览器中
所扮演的角色。

尽管 java.net 包提供了基础的功能，可以通过 HTTP 协议来访问网络资源，但它没有提供许多应用需要的灵活性或功能性。
HttpClient 通过高效，即时，并且富有特色的包填补 `java.net` 这一空缺，实现了 HTTP 绝大部分的标准和建议的客户端。

用于扩展，并为基础 HTTP 协议提供鲁棒性支持的 HttpClient 将受到那些构建 HTTP 客户端应用的用户青睐，
例如 web 浏览器，网络服务客户端，或者是运用或扩展 HTTP 协议用于分布式会议。

#### HttpClient 适用范围

- 客户端 HTTP 传输库基于 [HttpCore](http://hc.apache.org/httpcomponents-core-ga/index.html)
- 基于传统（阻塞式）I/O
- 内容无关

#### HttpClient 不是什么

- HttpClient 不是浏览器，它是客户端的 HTTP 传输库。它的目的在于发送、接受 HTTP 信息。如果没有明确设置，或者
重新格式化请求 / 重写定位 URI， 或其它不涉及 HTTP 传输的功能，HttpClient 不会去
解析内容，运行嵌入在 HTML 页面的 javascript 代码，获取内容类型（content type）。 


---

### 基础

#### 1.1) 执行请求 

`HttpClient` 中最重要的功能是执行 HTTP 方法。执行 HTTP 方法包含一个或多个 HTTP 请求/响应交换，
通常是在 `HttpClient` 内部处理。使用者需要提供一个要执行的请求对象，`HttpClient` 会向目标服务器发送请求，
返回相应的响应对象，或者在执行失败时抛出异常。

`HttpClient` API 的主要入口是 `HttpClient` 接口，它定义了上面所述的规则。

这里有一个关于请求执行过程的例子，格式很简单：
```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.excute(httpget);
try {
    // TODO
} finally {
    response.close();
}
```

1. HTTP 请求

所有的 HTTP 请求都有一个请求行，包含一个方法名，请求 URI 和 HTTP 协议版本。

HttpClient 支持所有的定义在 HTTP/1.1 规范的 HTTP 原装方法： GET, HEAD, POST, PUT, DELETE, TRACE 和 OPTIONS。
每一个方法都有相应的类：HttpGet, HttpHead, HttpPost, HttpPut, HttpDelete, HttpTrace 和 HttpOptions。

请求 URI 是通用资源标识符，用来定位要响应请求的资源。HTTP 请求的 URI 包含协议，主机名，可选的端口号，资源路径，
可选的查询字符串和可选的片段。

```
HttpGet httpget = new HttpGet("http://www.google.com/search?hl=en&q=httpclient&btnG=Google+Search&aq=f&oq=");
```

HttpClient 提供了 `URIBuilder` 工具类来简化请求 URI 的创建和修改。
```
URI uri = new URIBuilder()
        .setScheme("http")
        .setHost("www.google.com")
        .setPath("/search")
        .setParameter("q", "httpclient")
        .setParameter("btnG", "Google Search")
        .setParameter("aq", "f")
        .setParameter("oq", "")
        .build();
HttpGet httpget = new HttpGet(uri);
System.out.println(httpget.getURI());
```

stdout >
```
http://www.google.com/search?hl=en&q=httpclient&btnG=Google+Search&aq=f&oq=
```

2. HTTP 响应

HTTP 响应是服务器在收到并解析了客户端的请求信息后返回给客户端的信息。消息的第一行包含了协议的版本号，后面有一个数字
表示的状态码，还有一个相关的词语。

```
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK);

System.out.println(response.getProtocolVersion());
System.out.println(response.getStatusLine().getStatusCode());
System.out.println(response.getStatusLine().getReasonPhrase());
System.out.println(response.getStatusLine().toString());
```

stdout >
```
HTTP/1.1
200
OK
HTTP/1.1 200 OK
```

3. 使用消息头

HTTP 消息可以包含一些用来描述像 `content length`, `content type` 这些的头部信息。HttpClient 提供了检索、
添加、移除和枚举头部的方法。

```
HttpResponse response = enw BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie", "c1=a; path=/; domain=localhost");
response.addHeader("set-Cookie", "c2=b; path=\"/\", c3=c; domain=\"localhost\"");
Header h1 = response.getFirstHeader("Set-Cookie");
System.out.println(h1);
Header h2 = response.getLastHeader("Set-Cookie");
System.out.println(h2);
Header[] hs = response.getHeaders("Set-Cookie");
System.out.println(hs.length);
```

stdout >
```
Set-Cookie: c1=a; path=/; domain=localhost
Set-Cookie: c2=b; path="/", c3=c; domain="localhost"
2
```

用 HeaderIterator 接口来获取给定类型的所有头部是最高效的方式。

```
HttpResponse response = enw BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie", "c1=a; path=/; domain=localhost");
response.addHeader("set-Cookie", "c2=b; path=\"/\", c3=c; domain=\"localhost\"");

HeaderIterator it = response.headerIterator("Set-Cookie");

while(it.hasNext()) {
    System.out.println(it.next());
}
```

stdout >
```
Set-Cookie: c1=a; path=/; domain=localhost
Set-Cookie: c2=b; path="/", c3=c; domain="localhost"
```

迭代器同时提供了方便的方法来将 HTTP 消息解析成独立的头部元素。

```
HttpResponse response = enw BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie", "c1=a; path=/; domain=localhost");
response.addHeader("set-Cookie", "c2=b; path=\"/\", c3=c; domain=\"localhost\"");

HeaderElementIterator it = new BasicHeaderElementIterator(response.headerIterator("Set-Cookie"));

while(it.hasNext()) {
    HeaderElement elem = it.nextElement();
    System.out.println(elem.getName() + " = " + elem.getValue());

    NameValuePair[] params = elem.getParameters();
    for(int i = 0; i < params.length; i++) {
        System.out.println(" " + params[i]);
    }
}
```

stdout >
```
c1 = a
path=/
domain=localhost
c2 = b
path=/
c3 = c
domain=localhost
```

4. HTTP 实体

HTTP 消息能够携带与请求或响应相关的内容实体。这些实体能在某些请求和某些响应中找到，因为它们是可选的。
使用实体的请求指的是封装请求的实体。HTTP 协议规定了两种实体封装请求方法：POST 和 PUT。
响应通常被认为是封装内容的实体。当然这个规定也有例外，比如 HEAD 方法的响应和 
204 No Content，304 Not Modified， 205 Reset Content 响应

HttpClient 能按照它们的内容来源分辨出三种实体：
- streamed:  该内容是从一个流中获取的，或者是突然生成的。特别的，这一类包含了正被 HTTP 响应接受的实体。
流式实体通常不能复现。
- self-contained:  内存中的，或者不是通过连接及其它实体获得的内容。自包含（Self-Contained）实体一般可以复现。
这一类实体绝大多数是用于封装 HTTP 请求的实体。
- wrapping:  从其它实体中获取的内容

当从 HTTP 响应发出内容时，这个分类对管理连接就很重要了。对于应用生成的那些请求实体，它们只用 HttpClient 发送，
流式（Streamed）和自包含（self-contained）之间的区别就不那么重要了。这时，建议将不重复的实体划为流式（streamed），
可重复的实体划为自包含的（self-contained）。

===

使用可重复的实体

由于一个实体既可以表示二进制内容，又能表示字符内容，因此它支持字符编码（为了支持后者，即字符内容）。

当发送封装有内容的请求或者成功收到请求后响应内容要被返回给客户端时，实体就会被创建。

要从实体中读出内容，一种方法是通过 `HttpEntity#getContent()` 方法查询输入流，`getContent()` 方法返回
一个 `java.io.InputStream` 对象。另一种方法是提供一个输出流给 `HttpEntity#writeTo(OutputStream)` 方法，
这会一次性返回所有被写入给定流内的内容。

通过传入的信息接收到实体后，`HttpEntity#getContentType()` 和 `HttpEntity#getContentLength()` 方法可以用来读出
大多数元数据，比如 `Content-Type` 和 `Content-Length` 头（如果能够获取的话）。因为 `Content-Type` 头可以包含
文本多媒体类型的字符编码，像 text/plain 或者是 text/html，`HttpEntity#getContentEncoding()` 方法可以读出这些信息。
如果无法获取头部信息的话，返回的长度就会是 -1 ，类型是 NULL。如果 `Content-Type` 头部可获得，那么将返回
一个 Header 对象。

要发送消息时，创建一个实体，需要向实体创建者提供元数据。

```
StringEntity myEntity = new StringEntity("important message", ContentType.create("text/plain", "UTF-8"));

System.out.println(myEntity.getContentType());
System.out.println(myEntity.getContentLength());
System.out.println(EntityUtils.toString(myEntity));
System.out.println(EntityUtils.toByteArray(myEntity).length);
```

stdout >
```
Content-Type: text/plain; charset=utf-8
17
important message
17
```

5. 确保基础资源的释放

为了确保系统资源的合理释放，必须关闭与实体相关联的内容流或者响应。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.excute(httpget);
try {
    HttpEntity entity = response.getEntity();
    if(entity != null) {
        InputStream instream = entity.getContent();
        try {
            // do something useful
        } finally {
            instream.close();
        }
    }
} finally {
    response.close();
}
```

关闭内容流和关闭响应的区别在于前者会试着通过消耗实体内容来保持基础连接，而后者会立即切断并中止连接。

请注意，一旦实体被完全写入到输出流中后， `HttpEntity#writeTo(OutputStream)` 方法同样需要保证系统资源的释放。
如果这个方法要用到一个调用 `HttpEntity#getContent()` 方法得到的 `java.io.InputStream` 的实例，那么这个实例也应该在 
finally 块中关闭。

使用流式实体时，可以调用 `EntityUtils#consume(HttpEntity)` 方法来保证实体内容被完全消耗，也关闭了基础连接。

但也可能某些情况，当整个响应内容的一小部分需要被检索，而消耗剩余内容及连接可复用造成的性能代价实在是太高了，
这时就要关闭响应来中止内容流。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.excute(httpget);
try {
    HttpEntity entity = response.getEntity();
    if(entity != null) {
        InputStream instream = entity.getContent();
        int byteOne = instream.read();
        int byteTwo = instream.read();
        // Do not need the rest
    }
} finally {
    response.close();
}
```

连接不会被复用，但是其中包含的所有资源将会被正确释放。

6. 消耗实体内容

推荐一种消耗实体内容的方式，使用 `HttpEntity#getContent()` 或 `HttpEntity#writeTo(OutputStream)` 方法。
`HttpClient` 配合 `EntityUtils` 类，这个类用几个静态方法，能让读取实体中的内容或信息更加简单。
不是直接读取 `java.io.InputStream` 对象，你可以用这个类的某些方法，用字符串或是字节数组来检索整个内容主题。
但是强烈不推荐使用 `EntityUtils` 类，除非响应实体来源于可信的 HTTP 服务器，并且知道它的最大长度。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.excute(httpget);
try {
    HttpEntity entity = response.getEntity();
    if(entity != null) {
            long len = entity.getContentLength();
            if(len != -1 && len < 2048) {
                System.out.println(EntityUtils.toString(entity));
            } else {
                // Stream content out
            }
    }
} finally {
    response.close();
}
```

有时候多次读取实体内容很必要。这是实体内容就需要用某种方式缓存着，要么在内存里，要么在硬盘里。
最简单的方式是将原始实体用 `BufferedHttpEntity` 类封装。这就会把原始实体的内容放在内存缓冲区里。
在其它任何方面，实体封装器都有一个原始的实体。

```
CloseableHttpResponse response = httpclient.excute(httpget);
HttpEntity entity = response.getEntity();
if(entity != null) {
    entity = new BufferedHttpEntity(entity);
}
```

7. 产生实体内容

`HttpClient` 提供了几个类，它们能够通过 HTTP 连接来高效地输出内容。这些类的实例可以与封装请求的
实体（比如 `POST` 和 `PUT`）关联，可以把实体内容封装到输出的 HTTP 请求中。 `HttpClient` 提供几个类，
适用于大多数常见的数据，诸如字符串，字节数组，输入流和文件： `StringEntity`, `ByteArrayEntity`, 
`InputStreamEntity` 和 `FileEntity`。

```
File file = new File("somefile.txt");
FileEntity entity = new FileEntity(file, ContentType.create("text/plain", "UTF-8"));

HttpPost httppost = new HttpPost("http://localhost/action.do");
httppost.setEntity(entity);
```

注意 `InputStreamEntity` 不可复用，因为它仅能从基础数据流读取一次。一般来说推荐实现一个自定义的 HttpEntity 类
是一个良好的开头，这个类是自包含的而不是使用普通的 `InputStreamEntity`, `FileEntity`。

===

HTML 表单

许多应用需要模拟提交 HTML 表单，例如，登陆到一个网络应用或者提交输入的内容。HttpClient 提供 
UrlEncodedFormEntity 实体类来帮助处理。

```
List<NameValuePair> formparams = new ArrayList<NameValuePair>();
formparams.add(new BasicNameValuePair("param1", "value1"));
formparams.add(new BasicNameValuePair("param2", "value2"));
UrlEncodedFormEntity entity = new UrlEncodedFormEntity(formparams, Consts.UTF_8);
HttpPost httppost = new HttpPost("http://localhost/handler.do");
httppost.setEntity(entity);
```

UrlEncodedFormEntity 实例会用所谓的 URL 编码来编码参数，产生以下内容：

```
param1=value1&param2=value2
```

===

内容块

一般来讲推荐让 HttpClient 根据发送的 HTTP 消息属性来选择最合适的转码。但也有可能要告诉 HttpClient 某个块优先编码，
将 `HttpEntity#setChunked()` 为 true 就行。要注意 HttpClient 仅会把这个标志当作提示。当设置的 HTTP 
协议版本不支持块编码时，这个值会被忽略掉，比如 HTTP/1.0。

```
StringEntity entity = new StringEntity("important message", ContentType.create("plain/text", Consts.UTF_8));
entity.setChunked(true);
HttpPost httppost = new HttpPost("http://localhost/action.do");
httppost.setEntity(entity);
```

8. Response handlers

最简单、最方便的处理响应的方式是使用 `ResponseHandler` 接口，它包含了 `handleResponse(HttpResponse response)` 方法。
这个方法彻底的解决了用户关于连接管理的担忧。使用 `ResponseHandler` 时，HttpClient 会自动处理连接，无论执行请求成功
或是发生了异常，都确保连接释放到连接管理者。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/json");

ResponseHandler<MyJsonObject> rh = new ResponseHandler<MyJsonObject>() {

    @Override
    public JsonObject handleResponse(final HttpResponse response) throws IOException {
        StatusLine statusLine = response.getStatusLine();
        HttpEntity entity = response.getEntity();
        if(statusLine.getStatusCode() >= 300) {
            throw new ClientProtocolException("Response contains no content");
        }

        Gson gson = new GsonBuilder().create();
        ContentType contentType = ContentType.getOrDefault(entity);
        Charset charset = contentType.getCharset();
        Reader reader = new InputStreamReader(entity.getContent(), charset);
        return gson.fromJson(reader, MyJsonObject.class);
    }
};
MyJsonObject myjson = client.execute(httpget, rh);
```

#### 1.2) HttpClient 接口

HttpClient 接口代表了执行 HTTP 请求的最重要的规则。它利用请求执行过程中的无限制或特殊细节，
把特定的连接管理，状态管理，授权和重定向处理留给了不同的实现者。这更容易用别的功能（比如缓存响应内容）来完善接口。

一般来说 `HttpClient` 的实现是充当外观，暴露一些特殊意图的处理者或者策略接口，来合理的处理实现 HTTP 协议中特殊的
部分，比如重定向、认证或确定连接持久化和持续活跃。这保证了用户可以选择使用自定义的，应用指定的实现方式来替换
默认的实现方式。

```
ConnectionKeepAliveStrategy keepAliveStrat = new DefaultConnectionKeepAliveStrategy() {

    @Override
    public long getKeepAliveDuration(HttpResponse response, HttpContext context) {
        long keepAlive = super.getKeepAliveDuration(respnse, context);
        if(keepAlive == -1) {
            // Keep connections alive 5 secondes if a keep-alive value
            // has not be explicitly set by the server
            keepAlive = 5000;
        }
        return keepAlive;
    }
};
CloseableHttpClient httpclient = HttpClients.custom().setKeepAliveStrategy(keepAliveStrat).build();
```

1. HttpClient 线程安全

`HttpClient` 的实现应该是线程安全的，建议在执行多次请求时复用同一个 `HttpClient` 对象。

2. HttpClient resource deallocation

当不再需要 `CloseableHttpClient` 的实例时，并且要离开作用域时，与之相关联的连接管理器必须
要通过 `CloseableHttpClient#close()` 方法关闭。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
try{
    // TODO
    // <some code>
} finally {
    httpclient.close();
}
```

#### 1.3) HTTP 运行的上下文

原声的 HTTP 被设计为弱状态，请求-响应式的协议。但实际应用常常需要能够通过几个逻辑相关的请求-响应交换
来持久化状态信息。为了让应用能保持在处理中的状态，`HttpClient` 允许在一个特殊的运行上下文中执行 HTTP 请求，
这就是 HTTP 运行时上下文。如果在连续请求时复用同一个上下文，能把各种逻辑相关的请求加入到一个逻辑会话中。
HTTP 上下文的作用类似于 `java.util.Map<String, Object>`。它仅是一个键值对集合。应用可以设置上下文中的属性，
优先执行请求或者在执行完毕后检验上下文。

`HttpContext` 能包含任意的对象，可能在多线程下共享工作时不太安全。建议每个执行线程都保持自己的上下文。

在[执行 HTTP 请求](#执行请求) 这一节里，`HttpClient` 向执行上下文中添加了以下属性：
- HttpConnection  代表与目标服务器实际连接的实例
- HttpHost  代表目标主机的实例
- HttpRoute  代表完整的连接路由的实例
- HttpRequest  代表实际的 HTTP 请求的实例。执行上下文中的 `HttpRequest` 对象总是表示明确的消息状态，因为它
是发送到服务器中的。每个默认的 HTTP/1.0 和 HTTP/1.1 协议使用相对应的 URI。但如果请求是是通过非通道模式的代理发送的，
这个 URI 就是绝对的。
- HttpResponse  代表实际 HTTP 响应的实例
- java.lang.Boolean  代表实际请求是否被完全发送到目标主机的标志的对象
- RequestConfig  代表实际的请求配置的对象
- java.util.List<URI>  代表在执行请求过程中的所有重定向的地址的对象

可以用 `HttpClientContext` 适配器类简化与上下文状态的交互。

```
HttpContext context = <...>
HttpClientContext clientContext = HttpClientContext.adapt(context);
HttpHost target = clientContext.getTargetHost();
HttpRequest request = clientContext.getRequest();
HttpResponse response = clientContext.getResponse();
RequestConfig config = clientContext.getRequestConfig();
```

许多代表同一个逻辑关联会话的请求队列应该在同一个 `HttpContext` 实例下执行，确保在请求间自动传播会话上下文和状态信息。

下面的例子中，初始请求设置的请求配置将会被保存到运行时上下文中，并会被应用到该上下文中的其它请求。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
RequestConfig requestConfig = RequestConfig.custom()
        .setSocketTimeout(1000)
        .setConnectTimeout(1000)
        .build();

HttpGet httpget1 = new HttpGet("http://localhost/1");
httpget1.setConfig(requestConfg);
CloseableHttpResponse response1 = httpclient.excute(httpget1, context);
try{
    HttpEntity entity1 = response1.getEntity();
} finally {
    response1.close();
}
HttpGet httpget2 = new HttpGet("http://localhost/2");
CloseableHttpResponse response2 = httpclient.excute(httpget2, context);
try {
    HttpEntity entity2 = response2.getEntity();
} finally {
    response.close();
}
```

#### 1.4) HTTP 协议截获器

HTTP 协议截获器是实现了 HTTP 协议特殊方面的程序。通常认为协议截获器作用在接收到的消息的特殊的头部或
一组相关联的头部中，或者是用特殊的头部或一组相关的头部构成将要发送的消息。协议截获器可以操作封装了消息透明的
内容实体，实现压缩/解压缩就是一个不错的例子。通常这是通过装饰模式来完成的，用包含实体的类来装饰原始的实体。
几个协议截获器能够组合形成一个逻辑单元。

协议截获器可以通过共享信息来协同工作，比如通过 HTTP 运行上下文共享处理状态。协议截获器能够用 HTTP 上下文来给
一个或几个连续的请求存储状态信息。

协议截获器必须是线程安全的。类似 servlets，协议截获器不应该使用实例变量，除非这些变量的访问是同步的。

这是关于如何使用局部上下文在连续的请求间，持久化处理状态：

```
CloseableHttpClient httpclient = HttpClients.custom()
        .addInterceptorLast(new HttpRequestInterceptor(){
            public void process(final HttpRequest request, final HttpContext context) 
                    throws HttpException, IOException {
                        AtomicInteger count = (AtomicInteger) context.getAttribute("count");
                        request.addHeader("Count", Integer.toString(count.getAndIncrement()));
            }
        })
        .build();

AtomicInteger count = new AtomicInteger();
HttpClientContext localContext = HttpClientContext.create();
localContext.setAttribute("count", count);

HttpGet httpget = new HttpGet("http://localhost/");
for (int i = 0; i < 10; i++) {
    CloseableHttpResponse response = httpclient.execute(httpget, localContext);
    try {
        HttpEntity entity = response.getEntity();
    } finally {
        response.close();
    }
}
```

#### 1.5) 处理异常

HTTP 协议处理能够抛出两种类型的异常：如果发生了 I/O 错误，抛出 `java.io.IOException` 异常，比如连接超时或者
连接重置，如果发生了 HTTP 错误，抛出 `HttpException` ，比如违反了 HTTP 协议。通常认为 I/O 错误无关紧要并且
可以修复，而认为 HTTP 协议错误是致命的并且不能自动修复。请注意 `HttpClient` 的实现是像 `ClientProtocolException` 
一样重复抛出 `HttpException`， `ClientProtocolException` 是 `java.io.IOException` 的子类。这保证 `HttpClient`
的用户在一个 catch 块中既能处理 I/O 错误，也能处理违反协议的错误。

1. HTTP 传输安全

了解 HTTP 协议并不适合所有类型的应用很重要。HTTP 是一个简单的面向请求/响应式的协议，一开始就设计成支持检索
静态和动态生成的内容。它从未想过去支持事务型操作。例如，如果 HTTP 服务器成功接收和处理了请求，它将会按照服务端的
协议，生成响应，返回状态码给客户。即便客户端由于读入超时、取消请求或者系统崩溃而没能收到响应的全部内容，服务端也
不会尝试回滚事务。如果客户端要重试同样的请求，服务器将不可避免地结束执行多次同样的事务。有时候这将导致应用数据错误
或者应用状态不一致。

即使 HTTP 从没被设计成支持事务处理的协议，如果条件允许，它还是能用作重要任务的传输协议。确保 HTTP 传输层安全，
系统就要确保传输层的 HTTP 方法的幂等性。

2. 幂等方法

HTTP/1.1 关于幂等方法的定义

> 方法同样可以有幂等属性（除了错误或过期等方面），N 的副作用 > 0 ，同样的请求被视作一个请求。

换句话说，应用应该保证它能弄清楚一个方法的各种运行结果的含义。这可以做到，例如，提供一个独特的事务 id ，用其它
方法避免运行同样的裸机操作。

请注意这个问题不是 HttpClient 独有的。基于浏览器的应用都会面临同样的与 HTTP 方法非幂等性相关的问题。

HttpClient 默认认为无实体封装的方法比如 `GET` 和 `HEAD` 是幂等性的，而 `POST` 和 `PUT` 由于兼容性就不是。

3. 自动解决异常

HttpClient 默认会尝试自动解决 I/O 异常。默认的自动解决机制仅限于解决一些已知是安全的异常。

- HttpClient 不会尝试解决逻辑错误或者 HTTP 协议错误（源自 `HttpException` 类的异常）
- HttpClient 会自动重试幂等性的方法。
- HttpClient 会自动重试那些由于传输异常导致的方法，尽管 HTTP 请求仍然被发送到目标服务器上（比如请求还没有
完全发送到服务器）

4. Request retry handler

为了保证自定义的异常处理机制，应该实现 `HttpRequestRetryHandler` 接口。

```
HttpRequestRetryHandler myRetryHandler = new HttpRequestRetryHandler() {
    public boolean retryRequest (IOException exception, int executionCount, HttpContext context) {
        if(executionCount >= 5) {
            // Do not retry if over max retry count
            return false;
        }
        if(exception instanceof InterruptedIOException) {
            // Timeout
            return false;
        }
        if(exception instanceof UnknownHostException) {
            // Unknown host
            return false;
        }
        if(exception instanceof ConnectTimeoutException) {
            //Connection refused
            return false;
        }
        if(exception instanceof SSLException) {
            // SSL handshake exception
            return false;
        }

        HttpClientContext clientContext = HttpClientContext.adapt(context);
        HttpRequest request = clientContext.getRequest();
        boolean idempotent = !(request instanceof HttpEntityEnclosingRequest);
        if(!idempotent) {
            // Retry if the request is considered idempotent
            return true;
        }
        return false;
    }
};
CloseableHttpClient httpclient = HttpClients.custom()
        .setRetryHandler(myRetryHandler)
        .build();
```

请注意可以使用 `StandardHttpRequestRetryHandler` 代替默认使用的处理器，自动重试那些根据 RFC-2616 定义为幂等性的
请求方法： `GET`,`HEAD`,`PUT`,`DELETE`,`OPTIONS` 和 `TRACE`。


#### 1.6) 终止请求

有些情况，由于目标服务器的高负载或者很多并发请求发送给客户端，导致 HTTP 请求没能在预期的时间内完成。这时就有必要
提前终止请求并且解除执行线程对一个 I/O 操作的占用。HTTPClient 执行的 HTTP 请求可以在任何阶段被打断，调用 
`HttpUriRequest#abort()` 方法就可以了。这个方法是线程安全的，能被任何线程调用。当一个 HTTP 请求被终止了，它的
执行线程哪怕当时占用了一个 I/O 操作，也能确保解锁并抛出 `InterruptedIOException` 异常。

#### 1.7) 处理重定向

`HttpClient` 自动处理所有类型的重定向，除了某些 HTTP 规则因需要用户干预而明令禁止的类型。参见其它重定向（状态码：303），
 HTTP 规则要求将 `POST` 和 `PUT` 请求被转化为 `GET` 请求。可以使用自定义的重定向策略，利用 HTTP 规则来解除 
POST 方法自动重定向的限制。

```
LaxRedirectStrategy redirectStrategy = new LaxRedirectStrategy();
CloseableHttpClient httpclient = HttpClients.custom()
        .setRedirectStrategy(redirectStrategy)
        .build();
```

HttpClient 一般能在执行过程中重写请求消息，每个默认的 HTTP/1.0 和 HTTP/1.1 一般用相关联的请求 URI。同样的，
原始请求可能会被多次重定向到其它地方。最终解译出的 HTTP 绝对地址可以用原始请求和上下文来构建。协助方法 `URIUtils#resolve`
 可以用来构建解译的绝对 URI，从而生成最终的请求。这个方法包含了在重定向请求或者原始请求中的最后一个分片校验器。

```
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpClientContext context = HttpClientContext.create();
HttpGet httpget = new HttpGet("http://localhost:8080/");
CloseableHttpResponse respnse = httpclient.excute(httpget, context);
try {
    HttpHost target = context.getTargetHost();
    List<URI> redirectLocations = context.getRedirectLocations();
    URI location = URIUtils.resolve(httpget.getURI(), target, redirectLocations);
    System.out.println("Final HTTP location: " + location.toASCIIString());
    // Expected to be an absolute URI
} finally {
    response.close();
}
```

----

### 连接管理器

#### 2.1) 连接持久化

建立两个主机之间的连接的过程非常复杂，包含两个端点之间的许多包交换，这非常耗时。连接握手的开销很必要，尤其对于
短 HTTP 消息。如果打开的连接可以被复用，执行多个请求，可以产生很高的数据量。

HTTP/1.1 定义了 HTTP 连接默认可以被多个请求复用。符合 HTTP/1.0 标准的端点能用机制来明确交换他们的偏好以便
保持连接活跃，并用该连接发送请求。HTTP 代理能保持空闲连接活跃一段时间，以防需要用同一个目的主机的连接进行请求。

#### 2.2) HTTP 连接路由

HttpClient 能够直接建立到目的主机的连接，或者通过一个包含多个中间连接（也称做跃点）的路径。HttpClient 将一条路由的
连接分为简单的、隧道的、分层的。使用多个中间代理进行隧道连接到目标主机也被成为代理链。

简单路由仅通过一个代理连接到目标建立。隧道路由通过第一个连接，然后经一系列代理连接到目标。没有代理的路径无法进行
隧道连接。分层路由通过进行分层一个现有连接的协议进行连接。在连到目标主机的隧道中或者没有代理的直连中协议才能被分层。

1. 计算路由

`RouteInfo` 接口代表了信息

2. Secure HTTP connections

#### 2.3) HTTP connection managers

1. Managed connections and connection managers

2. Simple connection manager

3. Pooling connection manager

4. Connection manager shutdown

#### 2.4) Multithreaded request excution

#### 2.5) Connection socket factories

1. Secure socket layering

2. Integration with connection manager

3. SSL/TLS customization

4. Hostname verification

#### 2.6) HttpClient proxy configuration

----

### HTTP state management

#### 3.1) HTTP cookies

#### 3.2) Cookie specifications

#### 3.3) Choosing cookie policy

#### 3.4) Custom cookie policy

#### 3.5) Cookie persistence

#### 3.6) HTTP state management and execution context

----

### HTTP authentication

#### 4.1) User credentials

#### 4.2) Authentication schemes

#### 4.3) Credentials provider

#### 4.4) Preemptive authentication

#### 4.5) NTLM Authentication

1. NTLM connection persistence

#### 4.6) SPNEGO/Kerberos Authentication

1. SPNEGO support in HttpClient

2. GSS/Java Kerberos Setup

3. login.conf file

4. krb5.conf / krb5.ini file

5. Windows Specific configuration

----

### Fluent API

#### 5.1) Easy to use facade API

1. Response handling

----

### HTTP Caching

#### 6.1) General Concepts

#### 6.2) RFC-2616 Compliance

#### 6.3) Example Usage

#### 6.4) configuration

#### 6.5) Storage Backends

----

### Advanced topics

#### 7.1) Custom client connections

#### 7.2) Stateful HTTP connections

1. User token handler

2. Persistent stateful connections

#### 7.3) Using the FutureRequestExecutionService

1. Creating the FutureRequestExecutionService

2. Scheduling requests

3. Canceling tasks

4. Callbacks

5. Metrics