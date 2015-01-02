**Callbacks**

可以在某个 middleware 执行之前、之后，执行一些回调方法。

因为我们 middleware 本身就是链式调用，一个个执行，所以这里的回调意义不大。

```
define_callbacks :call
```

任何一个 middleware (或者说 Rack)很重要的两个方法：

```ruby
def initialize(app)
  @app = app
  # ...
end

def call(env)
  # ...
    @app.call(env)
  # ...
end
```
