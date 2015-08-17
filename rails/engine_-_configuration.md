## Configuration

Railtie、Engine、Application 都有自己的 Configuration 模块。

由于 Ruby 的继承机制，我们常用的 `config` 可以看作是它们中任意一个的实例对象。所以，理论上来说，它们提供的接口 `config` 都可直接调用。

#### 对外提供接口

实例方法：

```
paths
eager_load_paths
autoload_paths
autoload_once_paths

middleware
generators

root
root=
```

`generators` 使用举例：

```ruby
config.generators do |g|
  g.orm             :data_mapper, migration: true
  g.template_engine :haml
  g.test_framework  :rspec
end

# 或

config.generators.colorize_logging = false
```

`paths` 通过它可以查看 Engine 默认已经在用的路径，当我们定制 Engine 时，按照约定放置文件、目录是个好习惯。

`root`

```ruby
Rails.configuration.root == Rails.root
# => true
```

除 generators、root= 外，其余方法都是 get 获取数据，而不是 set 设置数据。

另，自定义的 Railtie 和自定义的 Engine，也可以对外提供 `config` 接口。
