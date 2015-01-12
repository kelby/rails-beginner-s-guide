## Callbacks 其它

- Rails 里的语法糖

  - ActiveRecord::Callbacks
  - ActiveModel::Callbacks#define_model_callbacks
  - ActiveSupport::Callbacks.define_callbacks

- 查看有什么回调

`spu._save_callbacks`

这里的 save 由 define_callbacks 声明，是回调，不是操作。

```ruby
define_model_callbacks :create, :update

# 同名方法
def create #:nodoc:
  run_callbacks(:create) { super }
end

# 同名方法
def update(*) #:nodoc:
  run_callbacks(:update) { super }
end

# 源代码
def define_model_callbacks(*callbacks)
  options = callbacks.extract_options!
  options = {
     :terminator => "result == false",
     :scope => [:kind, :name],
     :only => [:before, :around, :after]
  }.merge(options)

  types = Array.wrap(options.delete(:only))

  callbacks.each do |callback|
    define_callbacks(callback, options)

    types.each do |type|
      send("_define_#{type}_model_callback", self, callback)
    end
  end
end
```

为什么我们可以直接用 before_save, after_destroy 等方法，它们其实是语法糖，在 ActionModel::Callbacks 里有

`send("_define_#{type}_model_callback", self, callback)`
