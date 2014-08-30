## Caching

主要就是配置 ActionController 所使用的缓存。

配置举例(默认是 MemoryStore):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store, Memcached::Rails.new('localhost:11211')
config.action_controller.cache_store = MyOwnStore.new('parameter')
```

## Fragments

对片段缓存的一些操作。

你可以手动指定片段缓存过期的规则：

```ruby
expire_fragment(controller: 'products', action: 'recent', action_suffix: 'all_products')
```

提供了这么几个方法：

```ruby
fragment_cache_key

fragment_exist?

read_fragment
write_fragment

expire_fragment
```

操作这几个方法的是 controller，而这个几方法操作的是 cache_store.

> Note: ActionView::Helpers::CacheHelper 里的 cache 方法用到了 read_fragment、write_fragment 和 fragment_cache_key
