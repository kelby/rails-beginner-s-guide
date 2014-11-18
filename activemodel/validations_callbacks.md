## Validations 相关的 Callbacks

在执行校验之前、之后做校验。

提供 before_validation、after_validation 这两个和校验有关的回调方法。

```ruby
class MyModel
  include ActiveModel::Validations::Callbacks

  before_validation :do_stuff_before_validation
  after_validation  :do_stuff_after_validation
end
```

定义上，使用了 ActiveSupport 提供的回调相关方法；使用上，和 ActiveRecord 里的回调方法类似。
