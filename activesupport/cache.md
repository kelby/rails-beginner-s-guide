## Cache 缓存的源头

#### 类方法：

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
# => 返回一个 ActiveSupport::Cache::MemoryStore 实例对象

ActiveSupport::Cache.lookup_store(:mem_cache_store)
# => 返回一个 ActiveSupport::Cache::MemCacheStore 实例对象
```

#### Store

操作的对象是下面的 entry

是 FileStore, MemCacheStore, MemoryStore, NullStore 以及 LocalStore 的基类，某些方法需要子类重写才有意义。

**实例方法：**

```ruby
cleanup
clear

decrement
increment

delete
delete_matched

exist?

mute

read
write
read_multi
fetch
fetch_multi

silence!
```

`fetch` 使用举例：

```ruby
cache.write('today', 'Monday')
cache.fetch('today')  # => "Monday"

cache.fetch('city')   # => nil
cache.fetch('city') do
  'Duckburgh'
end
cache.fetch('city')   # => "Duckburgh"
```

其它实例方法：

```
key_matcher
```

**1) File Store (文件存储)**

```
cleanup
clear

decrement
increment

delete_matched
```

**2) Mem Cache Store (缓存存储)**

```
clear

read_multi

stats
```

**3) Memory Store (内存存储)**

```
cleanup
clear
prune

decrement
increment

delete_matched

pruning?
```

**4) ~~Null Store~~**

不使用任何介质进行存储，在开发、测试环境可能有用。

#### ~~Local Cache (本地缓存)~~

使用上述的几种介质进行存储，再快，也没有直接使用内存来得快。上述几种介质存储只要实现"本地缓存"，那么在一个 block 里首次调用变量时，会备份它到本地缓存，之后在 block 里再次调用(相同 cache key)，则直接使用本地缓起来存的，在这性能上比之前又更快了一点。

**Local Cache**

简单的 in-memory 缓存。

实例方法：

```
middleware

with_local_cache
```

其它实例方法：

```
set_cache_value
```

属于 middleware, rake middleware 里就能看到，并且一开始就执行了。

**Local Store**

简单的 memory 存储。(非线程安全)

```
clear

delete_entry
read_entry
write_entry
```

#### ~~Entry~~

表示某一条 cache 数据，包含了值和过期时间等信息。
