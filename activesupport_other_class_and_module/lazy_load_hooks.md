## Lazy Load Hooks

允许 Rails 延迟加载部分组件，从而加快应用的启动速度。

类方法：

```
on_load
run_load_hooks

execute_hook
```

使用举例：

```ruby
# on_load 方法声明
initializer 'active_record.initialize_timezone' do
  ActiveSupport.on_load(:active_record) do
    self.time_zone_aware_attributes = true
    self.default_timezone = :utc
  end
end
```

```ruby
# run_load_hooks 方法执行
ActiveSupport.run_load_hooks(:active_record, ActiveRecord::Base)
```

单独使用举例：

```ruby
class Color
  def initialize(name)
    @name = name

    ActiveSupport.run_load_hooks(:instance_of_color, self)
  end
end

ActiveSupport.on_load :instance_of_color do
  puts "The color is #{@name}"
end

Color.new("yellow")
# => "The color is yellow"
```
