## 在 Model 里快速定义 Callbacks

`define_model_callbacks(*callbacks)` 
一般的，ORM 都会提供大量的回调函数，并且有的回调函数之间还存在共性。直接使用 ActiveSupport 那一套回调机制的话，略显麻烦，还会有重复代码。所以，ActiveModel 封装了 ActiveSupport，使得定义回调函数变得简单方便。

使用举例：

```ruby
require 'active_model'

class Person
  extend ActiveModel::Callbacks
 
  define_model_callbacks :update
 
  # 运用 define_model_callbacks 自动生成的方法
  before_update :reset_me
  after_update  :say_success
 
  # 并不一定要和 define_model_callbacks 的名字一样，比如这里的示例代码
  # 但为了统一，易于理解，一般设置它们名字一样
  def update_me
    run_callbacks(:update) do
      p "update_me 真正要执行的代码"
      p "... before、after、around 就是针对这里的代码而言的！"
    end

    p "update_me 真正要执行的代码，还可以有其它内容"
    p "... 但为了简单、避免混淆，不建议再放其它代码！"
  end
 
  def reset_me
    p "在 update_me 真正要执行的代码之前，执行这里的代码"
  end
  
  def say_success
    p "在 update_me 真正要执行的代码之后，执行这里的代码"
  end
end

person = Person.new
person.update_me
```

上面例子里的 before_update 方法我们并没有定义。在使用 define_model_callbacks 后，自然的就为我们提供了 before_x、after_x、around_x 3 方法。
