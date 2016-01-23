## Callbacks - 快速提供 3 个回调方法

`define_model_callbacks(*callbacks)` 快速定义 Model 里使用的 3 个回调方法，它们是：before_x、around_x 和 after_x.

一般的，ORM 都会提供大量的回调函数，其中有的彼此之间还很相似。直接使用 Active Support 那一套回调机制的话，略显麻烦，还会有重复代码。所以，Active Model 封装了 Active Support，使得定义回调函数变得简单方便。

使用举例：

```ruby
require 'active_model'

class Person
  extend ActiveModel::Callbacks
 
  define_model_callbacks :update
 
  # 运用 define_model_callbacks 自动生成的方法
  before_update :reset_me
  # around_update ...
  after_update  :say_success

  # 为了统一，易于理解，这里的方法名应该用：update
  # 但这并不是规定死的，也可以不一致，比如这里的示例代码
  def update_me
    run_callbacks(:update) do
      p "update_me 真正要执行的代码"
      p "... before、around 和 after 就是针对这里的代码而言的！"
    end

    p "update_me 真正要执行的代码，还可以有其它内容"
    p "... 但为了简单、避免混淆，不建议再放其它任何代码！"
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

上面例子里的 `before_update` 和 `after_update` 方法我们并没有显式定义，是 define_model_callbacks 帮我们自动生成的。
