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

## Cache

`expand_cache_key(key, namespace = nil)`

Expands out the key argument into a key that can be used for the cache store. Optionally accepts a namespace, and all keys will be scoped within that namespace.

If the key argument provided is an array, or responds to to_a, then each of elements in the array will be turned into parameters/keys and concatenated into a single key. For example:

```ruby
expand_cache_key([:foo, :bar])               # => "foo/bar"
expand_cache_key([:foo, :bar], "namespace")  # => "namespace/foo/bar"
```

The key argument can also respond to cache_key or to_param.

`lookup_store(*store_option)`

Creates a new CacheStore object according to the given options.

If no arguments are passed to this method, then a new `ActiveSupport::Cache::MemoryStore` object will be returned.

If you pass a Symbol as the first argument, then a corresponding cache store class under the ActiveSupport::Cache namespace will be created. For example:

```ruby
ActiveSupport::Cache.lookup_store(:memory_store)
# => returns a new ActiveSupport::Cache::MemoryStore object

ActiveSupport::Cache.lookup_store(:mem_cache_store)
# => returns a new ActiveSupport::Cache::MemCacheStore object
```

Any additional arguments will be passed to the corresponding cache store class's constructor:

```ruby
ActiveSupport::Cache.lookup_store(:file_store, '/tmp/cache')
# => same as: ActiveSupport::Cache::FileStore.new('/tmp/cache')
```

If the first argument is not a Symbol, then it will simply be returned:

```ruby
ActiveSupport::Cache.lookup_store(MyOwnCacheStore.new)
# => returns MyOwnCacheStore.new
```

**MemoryStore**

A cache store implementation which stores everything into memory in the same process. If you're running multiple Ruby on Rails server processes (which is the case if you're using mongrel_cluster or Phusion Passenger), then this means that Rails server process instances won't be able to share cache data with each other and this may not be the most appropriate cache in that scenario.

This cache has a bounded size specified by the :size options to the initializer (default is 32Mb). When the cache exceeds the allotted size, a cleanup will occur which tries to prune the cache down to three quarters of the maximum size by removing the least recently used entries.

MemoryStore is thread-safe.

```
cleanup(options = nil)
Preemptively iterates through all stored keys and removes the ones which have expired.

clear(options = nil)

decrement(name, amount = 1, options = nil)
Decrement an integer value in the cache.

delete_matched(matcher, options = nil)

increment(name, amount = 1, options = nil)
Increment an integer value in the cache.

prune(target_size, max_time = nil)
To ensure entries fit within the specified memory prune the cache by removing the least recently accessed entries.

pruning?()
Returns true if the cache is currently being pruned.
```
