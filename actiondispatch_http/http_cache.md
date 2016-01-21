## Cache 之 Request

**HTTP Cache 里的 Request 部分。**如：检测 ETag、Cache-Control

一般来说，客户端(通常是浏览器)也有缓存机制，我们可以设置 ETag 和 Last-Modified 告诉它们资源是否已更新，缓存是否已过期。

一般来说，我们不会手动调用这里的方法。

**提供方法：** 

```
etag_matches?

fresh?

if_modified_since
if_none_match
if_none_match_etags

not_modified?
```
