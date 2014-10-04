# Cache

ActiveSupport::Cache::Strategy::LocalCache::Middleware 中间件，rake middleware 里就能看到。

它用到了 LocalCacheRegistry 和 LocalStore.

- 做缓存是很自然的事
- cache key 很重要
- 与其做缓存，不如修改逻辑

## 缓存无处不在！

外部请求 -- Web服务器 -- 应用服务器 -- 应用

外部请求 --(ETag)-- Web服务器 -- 应用服务器 -- 应用

外部请求 -- Web服务器 --Rack(middleware)-- 应用服务器 -- 应用

外部请求 -- Web服务器 -- 应用服务器 -- 应用 -- 数据库

外部请求 -- Web服务器 -- 应用服务器 -- 应用 --(内存)-- 数据库

---

ETag

可以避免 Rails 渲染和下载相同内容的页面，节省了服务器的计算和网络资源。
