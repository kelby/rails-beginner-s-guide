由两部分组成：Caching 和 Caching Fragments.

**页面相关的服务端缓存**

## Caching

想要关闭片段缓存，可以配置(开发环境下默认就是 false)：

```ruby
config.action_controller.perform_caching = false
```

**Caching stores**

配置存储方式 (默认是 MemoryStore ):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store,
                                         Memcached::Rails.new('localhost:11211')
```

---

主要就是配置 ActionController 所使用的缓存。

配置举例(默认是 MemoryStore):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store,
                                         Memcached::Rails.new('localhost:11211')
config.action_controller.cache_store = MyOwnStore.new('parameter')
```

---

页面缓存、action缓存都被干掉了，留下很好的片段缓存。

视图包含两类内容：Controller传递过来的实例变量(动态内容，必需手动设置)，本身的静态内容(自动设置)。
缓存要考虑这两种内容的更新。

还有就是视图里的显示是层层嵌套的，它们之间有时候是有关联的，这种情况也要考虑。

```
# 第一次访问
Cache digest for
app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197
Write fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197

# 第二次访问
Cache digest for
app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197

# post.cache_key
 => "posts/1-20140421062215459004000"

# 更改静态内容(默认)
Cache digest for
app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment
views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc
Write fragment
views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc

# 更改动态内容(在这里是update post)
Cache digest for
app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment
views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc
Write fragment
views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc

Read fragment
views/localhost:3000/posts/1?action_suffix=post1/1a3c7591dece4354ee7da69dfc12f246
Write fragment
views/localhost:3000/posts/1?action_suffix=post1/1a3c7591dece4354ee7da69dfc12f246

怎么生成的？
views/projects/123-20120806214154/7a1156131a6928cb0026877f8b749ac9
      ^class   ^id ^updated_at    ^template tree digest
```

如何才能完全手动管理缓存？

```ruby
def fragment_name_with_digest(name) #:nodoc:
  names  = Array(name.is_a?(Hash) ? controller.url_for(name).split("://").last : name)
  digest = Digestor.digest name: @virtual_path,
                           finder: lookup_context,
                           dependencies: view_cache_dependencies

  [ *names, digest ]
end
```

1. 不要使用动态内容做 key
2. 关闭默认的加密

也就是：

```
cache 'all_available_products', skip_digest: true
```

```
expire_fragment('all_available_products')
```
才有用。

> Note: 在 Controller 和 View 里写缓存相关的代码，这真的很丑陋。并且一个页面往往由很多元素组成，只能在它的 action 里管理显然很不方便。推荐 cells

## Caching Fragments

对片段缓存的一些操作。

你可以手动指定片段缓存过期的规则：

```ruby
expire_fragment(controller: 'products', action: 'recent',
                action_suffix: 'all_products')
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

> Note: ActionView::Helpers::CacheHelper 里的 cache 方法用到了 read_fragment、write_fragment 和 fragment_cache_key.

参考

[Cache Digests 最大化緩存策略](http://blog.xdite.net/posts/2012/09/02/cache-digest-new-strategy/)

[Speed Up With Rails Cache](http://rubyer.me/blog/2012/09/04/speed-up-with-rails-cache/)<br>
[Advanced Caching: Part 2 - Using Caching Strategies](http://hawkins.io/2012/07/advanced_caching_part_2-using_strategies/)<br>
[Rails Cache for dummies](http://www.codelearn.org/blog/rails-cache-with-examples)

