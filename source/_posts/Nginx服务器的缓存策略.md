---
title: Nginx服务器的缓存策略
date: 2019-01-03 22:56:04
tags: [Nginx,前端性能优化]
categories: 后端
---

Nginx使用**Last-Modified**、**ETag**、**Expires**，这样可利用客户端（例如浏览器）的缓存，减少http请求，提高页面响应速度。


## Last-Modified/If-Modified-Since
Last-Modified在Nginx中也是默认开启的

### Nginx中设置
```nginx
http {

  //开启
  add_header  Last-Modified $date_gmt;

  //关闭
  add_header  Last-Modified "";

}

```
有些数据随时都在变化。 CNN.com 的主页经常几分钟就更新。另一方面，Google.com 的主页几个星期才更新一次 (当他们上传特殊的假日 logo，或为一个新服务作广告时)。 Web 服务是不变的：通常服务器知道你所请求的数据的最后修改时间，并且 HTTP 为服务器提供了一种将最近修改数据连同你请求的数据一同发送的方法。

如果你第二次 (或第三次，或第四次) 请求相同的数据，你可以告诉服务器你上一次获得的最后修改日期：在你的请求中发送一个 If-Modified-Since 头信息，它包含了上一次从服务器连同数据所获得的日期。如果数据从那时起没有改变，服务器将返回一个特殊的 HTTP 状态代码 304，这意味着 “从上一次请求后这个数据没有改变”。这一点有何进步呢？当服务器发送状态编码 304 时，不再重新发送数据。您仅仅获得了这个状态代码。所以当数据没有更新时，你不需要一次又一次地下载相同的数据；服务器假定你有本地的缓存数据。

### 工作原理
服务器器端返回资源时，如果头部带上了了 last-modified，那么 资源下次请求时就会把值加⼊入到请求头 if-modified-since 中，服务器器可以对⽐比这个值，确定资源是否发⽣生变化，如果 没有发⽣生变化，则返回 304。

## ETag/If-None-Match
### 什么是Etag
当发送一个服务器请求时，浏览器首先会进行缓存过期判断。浏览器根据缓存过期时间判断缓存文件是否过期。

### Nginx中设置
在nginx中Etag是默认开启的
```nginx
http {
  //开启
  etag on;

  //关闭
  etag off;
}
```

### 工作原理
Etag由服务器端生成，一般为MD5的值。客户端通过If-None-Match这个条件判断请求来验证资源是否修改。请求一个文件的流程可能如下：
>第一次请求
1. 客户端发起 HTTP GET 请求一个文件。
2. 服务器处理请求，返回文件内容和一堆Header，当然包括Etag(例如"2e681a-6-5d044840")(假设服务器支持Etag生成和已经开启了Etag).状态码200

>第二次请求
1. 客户端发起 HTTP GET 请求一个文件，注意这个时候客户端同时发送一个If-None-Match头，这个头的内容就是第一次请求时服务器返回的Etag：2e681a-6-5d044840
2. 服务器判断发送过来的Etag和计算出来的Etag匹配，因此If-None-Match为False，不返回200，返回304，客户端继续使用本地缓存；

流程很简单，问题是，如果服务器又设置了Cache-Control:max-age和Expires呢，怎么办？
答案是同时使用，也就是说在完全匹配If-Modified-Since和If-None-Match即检查完修改时间和Etag之后，服务器才能返回304.(不要陷入到底使用谁的问题怪圈)


## Expires
HTTP头信息Expires（过期时间） 属性是HTTP控制缓存的基本手段，这个属性告诉缓存器：相关副本在多长时间内是新鲜的。过了这个时间，缓存器就会向源服务器发送请求，检查文档是否被修 改。几乎所有的缓存服务器都支持Expires（过期时间）属性

### Nginx中设置
```nginx
http {
  expires 3d;  
  # expires 必须设置这个让浏览器不缓存，过期时间才生效
  add_header Cache-Control no-cache;
}
```

### 工作原理
在http头中设置一个过期时间，在过期时间之前，浏览器的请求都不会发出，而是自动从缓存中读取文件。除非缓存被清空，或者强制刷新。**缺陷在于，服务器时间和客服端的时间可能存在不一致，所以Http/1.1加入了 cache-control 头来改进这个问题。**

## cache-control
设置过期的时间长度（秒），在这个时间范围内，浏览器请求都会直接读缓存。当 expires 和 cache-control 都存在时，cache-control的优先级更高。

## 缓存优先级
last-modified/if-modified **<** etag/if-none-match **<** expires **<** cache-control

## 总结
1. **Expires**是控制浏览器是否直接从浏览器缓存取数据还是重新发请求到服务器取数据。
2. **Last-Modified/If-Modified-Since**和**ETag/If-None-Match**是浏览器发送请求到服务器后判断文件是否 已经修改过，如果没有修改过就只发送一个304回给浏览器，告诉浏览器直接从自己本地的缓存取数据；如果修改过那就整个数据重新发给浏览器。

## 缓存机制图解
<img src="https://s2.ax1x.com/2019/07/04/ZNQ6a9.png" width="100%">

