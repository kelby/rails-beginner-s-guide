## Validations 相关的 Callbacks

在执行校验之前、之后做某事。

提供 `before_validation`、`after_validation` 这两个和校验有关的回调方法。

```ruby
class MyModel
  include ActiveModel::Validations::Callbacks

  before_validation :do_stuff_before_validation
  after_validation  :do_stuff_after_validation
end
```

定义上，使用了 Active Support 提供的回调相关方法；使用上，和 Active Record 里的回调方法类似。
