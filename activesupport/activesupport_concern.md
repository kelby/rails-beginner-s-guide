## Concern

可以方便快捷的扩展某个类或模块，并且处理了潜在的依赖问题。

作用有二：

1) 更简洁、明了的语法。
<br>
2) 更好的处理模块之间的依赖关系，规避潜在的模块之间的循环依赖。

约定：ClassMethods 和 class_methods 自动继承、实例方法自动包含、自动使用 class_eval.

InstanceMethods 后来又改进了，直接包含即可。

原来你需要手动写 included 或 extended 里面的代码，有实例方法、类方法的话，也要手动包含或继承；现在按照约定来即可。

以前：

```ruby
module M
  def self.included(base)
    base.extend ClassMethods

    base.class_eval do
      # 执行某些方法
      scope :disabled, -> { where(disabled: true) }

      # 执行某些方法
      include InstanceMethods
    end
  end

  # 定义类方法
  module ClassMethods
    # ...
  end

  # 定义实例方法
  def a_instance_method
    # ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    # ...
  end
end
```

现在：

```ruby
require 'active_support/concern'

module M
  extend ActiveSupport::Concern

  included do
    # 执行某些方法
    scope :disabled, -> { where(disabled: true) }

    # 执行某些方法
    include InstanceMethods
  end

  # 定义类方法
  module ClassMethods
    # ...
  end

  # 定义实例方法
  def a_instance_method
    # ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    # ...
  end
end
```

本模块源代码、及示例已经经过多次更改，多学语法，适应改变适应其变化。
