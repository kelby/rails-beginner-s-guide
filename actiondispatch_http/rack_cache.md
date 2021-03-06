## Rack Cache

Rack::Cache 也属于 Rack middleware，可用后端服务存储 assets(静态资源) 内容，默认放在 public/ 目录下。

现在默认没有采用：

```ruby
config.action_dispatch.rack_cache = false
```

你可以配置，如：

```ruby
Rails.configuration.action_dispatch.rack_cache
# => {:metastore=>"rails:/", :entitystore=>"rails:/", :verbose=>false}
```

Rails 对应有 Rails Meta Store 和 Rails Entity Store 类，它们都有：

```ruby
def initialize(store = Rails.cache)
  @store = store
end
```

并且都有对 @store 的 read、write 等方法。

链接 [rack-cache](https://github.com/rtomayko/rack-cache)
