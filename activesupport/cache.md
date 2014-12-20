## Cache 缓存的源头

`expand_cache_key(key, namespace = nil)`

让 cache_key 过期。

使用举例：

```ruby
expand_cache_key([:foo, :bar])               # => "foo/bar"
expand_cache_key([:foo, :bar], "namespace")  # => "namespace/foo/bar"
```

`lookup_store(*store_option)`

指定缓存存储方式。

使用举例：

```ruby
ActiveSupport::Cache.lookup_store(:memory_store)
# => returns a new ActiveSupport::Cache::MemoryStore object

ActiveSupport::Cache.lookup_store(:mem_cache_store)
# => returns a new ActiveSupport::Cache::MemCacheStore object
```

## Entry

write 的时候用到.

表示某一条 cache 数据，包含了值和过期时间。

## Store

操作的对象是下面的 entry

是 FileStore, MemCacheStore, MemoryStore, NullStore 以及 LocalStore 的基类
某些方法需要子类重写才有意义。

包含方法：

```ruby
cleanup, clear
decrement, delete, delete_matched
exist?
fetch, fetch_multi
increment
key_matcher
mute
read, read_multi
silence!
write
```

fetch 使用举例：

```ruby
cache.write('today', 'Monday')
cache.fetch('today')  # => "Monday"

cache.fetch('city')   # => nil
cache.fetch('city') do
  'Duckburgh'
end
cache.fetch('city')   # => "Duckburgh"
```

### File Store

```
read_entry
write_entry
```

### Mem Cache Store

```
build_mem_cache
read_multi
stats
```

### Memory Store

```
cached_size
prune, pruning?
```

## 默认缓存及存储

### Local Cache

简单的 in-memory 缓存。

```
middleware

set_cache_value
with_local_cache
```

属于 middleware, rake middleware 里就能看到，并且一开始就执行了。

### Local Store

简单的 memory 存储。(非线程安全)

```
clear

delete_entry
read_entry
write_entry
```
