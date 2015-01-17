**页面相关的服务端缓存**，由两部分组成：Caching 和 Caching Fragments.

## Caching

它几乎什么也没做，只是定义了 cache 同名方法，保证了执行缓存需要的两个条件：perform_caching = true 并且 cache_store 是合法的。

```ruby
def cache(key, options = {}, &block)
  if cache_configured?
    cache_store.fetch(ActiveSupport::Cache.expand_cache_key(key, :controller),
                      options, &block)
  else
    yield
  end
end
```

1) 想要关闭片段缓存，可以配置(开发环境下默认就是 false)：

```ruby
config.action_controller.perform_caching = false
```

2) Caching stores

配置怎么存储缓存(默认使用 MemoryStore):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store,
                                         Memcached::Rails.new('localhost:11211')
config.action_controller.cache_store = MyOwnStore.new('parameter')
```

缓存相关的其它事，自有人来处理，不用它操心。

## Caching Fragments

对片段缓存的一些操作。<br>
这里的方法属于中间处理过程，封装对底层的操作(也就是 cache_store)，然后提供接口给我们上层使用(也就是 cache 辅助方法)。

```
          cache 辅助方法
               |
               V
write_fragment & read_fragment 等
               |
               V
          cache_store
```

提供了这么几个方法：

```ruby
fragment_cache_key

fragment_exist?

read_fragment
write_fragment

expire_fragment
```

`expire_fragment` 你可以手动指定片段缓存过期的规则：

```ruby
expire_fragment(controller: 'products', action: 'recent',
                action_suffix: 'all_products')
```

操作这几个方法的是 controller，而这个几方法操作的是 cache_store.

> Note: ActionView::Helpers::CacheHelper 里的 cache 方法用到了 read_fragment、write_fragment 和 fragment_cache_key.

参考

[Cache Digests 最大化緩存策略](http://blog.xdite.net/posts/2012/09/02/cache-digest-new-strategy/)

[Speed Up With Rails Cache](http://rubyer.me/blog/2012/09/04/speed-up-with-rails-cache/)<br>
[Advanced Caching: Part 2 - Using Caching Strategies](http://hawkins.io/2012/07/advanced_caching_part_2-using_strategies/)<br>
[Rails Cache for dummies](http://www.codelearn.org/blog/rails-cache-with-examples)

