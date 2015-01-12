## Model Naming

Action Controller 和 Action View 里都有 Model Naming 模块。

Controller 和 View 经常要处理 record 对象，比如得到单数、复数格式，得到 cache_key、route_key、param_key 等，这都需要用到 Model 里的方法。此时，可以把 record 对象转化得到 model_name，然后再进行后续操作。

ActionController::ModelNaming 和 ActionView::ModelNaming 一样，都提供了：

```
convert_to_model

model_name_from_record_or_class
```

方便我们把对象转换成 Model 或 model_name 进行处理。
