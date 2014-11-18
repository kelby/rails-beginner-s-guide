## 在 Model 里快速定义 Callbacks

`define_model_callbacks(*callbacks)` 
一般的，ORM 都会提供大量的回调函数，并且有些回调函数还存在共性。直接使用 ActiveSupport 那一套回调机制的话，略显麻烦，还会有重复代码。所以，ActiveModel 封装了 ActiveSupport，使得定义回调函数变得简单方便。

使用举例：

```ruby
require 'active_model'

class Person
  extend ActiveModel::Callbacks
 
  define_model_callbacks :update
 
  before_update :reset_me
 
  def update
    run_callbacks(:update) do
      # This method is called when update is called on an object.
    end
  end
 
  def reset_me
    # This method is called when update is called on an object as a before_update callback is defined.
  end
end
```
