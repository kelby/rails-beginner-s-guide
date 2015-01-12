## HTTP Cache 之 Response

**HTTP Cache 里的 Response 部分。**如：设置 ETag、Cache-Control

一般来说，客户端(通常是浏览器)也有缓存机制，我们可以设置 ETag 和 Last-Modified 告诉它们资源是否已更新，缓存是否已过期。

一般来说，我们不会手动调用这里的方法。

### 

```
date
date=
date?

etag=

last_modified
last_modified=
last_modified?
```
