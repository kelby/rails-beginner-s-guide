## Concern

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
    ...
  end

  # 定义实例方法
  def a_instance_method
    ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    ...
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
    ...
  end

  # 定义实例方法
  def a_instance_method
    ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    ...
  end
end
```

作用有二：

1) 更简洁、明了的写法。(上面例子已说明)

2) 更好的处理模块之间的依赖关系。(官方文档有详细说明)
