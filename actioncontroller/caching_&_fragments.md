# Caching & Fragments

## Caching stores

可以配置 ActionController 所使用的缓存。

配置举例(默认是 MemoryStore is the default):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store, Memcached::Rails.new('localhost:11211')
config.action_controller.cache_store = MyOwnStore.new('parameter')
```

## cache

A method for caching fragments of a view rather than an entire action or page. This technique is useful caching pieces like menus, lists of news topics, static HTML fragments, and so on. This method takes a block that contains the content you wish to cache. See `ActionController::Caching::Fragments` for more information.

```erb
<% cache do %>
  <%= render "shared/footer" %>
<% end %>
```

and you can expire it using the `expire_fragment` method, like so:

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

> **Note:** 上面的 cache 方法用到了 read_fragment、write_fragment 和 fragment_cache_key<br/>
> **Note:** 用来防"爬虫"，很有效<br>

可以避免 Rails 渲染和下载相同内容的页面，节省了服务器的计算和网络资源。

和其它缓存的 cache key 一样，设置 ETag 的值很重要，也很讲究。
