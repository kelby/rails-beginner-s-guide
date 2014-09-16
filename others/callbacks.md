# Callbacks

Callbacks 跨度比较大。

----------

源头 ActiveSupport::Callbacks 的
define_callbacks
run_callbacks
set_callback
等

中游 ActiveModele::Callbacks 的
define_model_callbacks

下游
ActiveModele::Callbacks 的
valid? (校验相关)

ActiveRecord::Callbacks 的

    :initialize, :find, :touch, :only => :after
    :save, :create, :update, :destroy
对应的是 Model 里的回调(或过滤)


AbstractController::Callbacks 的

    before_action
    after_action
    around_action
    
    # 以及
    prepend
    skip
    append
对应的是 Controller 里的回调(或过滤)

ActiveJob::Callbacks 的

```ruby
before_enqueue
around_enqueue
after_enqueue

before_perform
around_perform
after_perform
```
