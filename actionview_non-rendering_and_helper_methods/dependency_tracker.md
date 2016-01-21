#### ~~Dependency Tracker~~

供 Digestor 使用，digest 的时候可以以数组的形式传 dependencies 作为其可选参数。

提供类方法：

```ruby
register_tracker
remove_tracker

find_dependencies
```

以数组的形式返回其依赖的模板（模板、局部模板、指定 layout 的模板）的名字：

```ruby
def dependencies
  render_dependencies + explicit_dependencies
end
```

一个 action 模板只有一个，所以主要针对的是里面所包含的局部模板。
