## 默认片段缓存策略

视图包含两类内容：Controller 传递过来的实例变量(动态内容，必需手动设置)，本身的静态内容(自动设置)。
缓存要考虑这两种内容的更新。

还有就是视图里的显示是层层嵌套的，它们之间有时候是有关联的，这种情况也要考虑。

怎么生成的 Cache Key？默认策略是路径 + 动态内容 Cache Key + 静态内容 Cache Key：

```
views/projects/123-20120806214154/7a1156131a6928cb0026877f8b749ac9
      ^class   ^id ^updated_at    ^template tree digest
```

第一次访问(读缓存失败，写缓存)

```
Cache digest for
app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197
Write fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197
```
·
第二次访问(读缓存成功)

```
Cache digest for
app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment
views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197
```

查看一下动态内容的 cache_key，方便对比

```
# post.cache_key
 => "posts/1-20140421062215459004000"
```

更改静态内容(读缓存失败，写缓存；动态内容这部分 Cache Key 不变)

```
Cache digest for
app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment
views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc
Write fragment
views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc
```

更改动态内容(在这里是update post. 读缓存失败，写缓存；静态内容这部分 Cache Key 不变)

```
Cache digest for
app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment
views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc
Write fragment
views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc
```

如何才能完全手动管理缓存？(也许永远都没必要这么做)：

1. 不要使用动态内容做 key
2. 关闭静态内容加密

比如：

```ruby
cache 'all_available_products', skip_digest: true
```

此时：

```ruby
expire_fragment('all_available_products')
```

才会生效。
