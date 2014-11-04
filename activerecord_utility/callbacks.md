# Callbacks

基于 ActionModel 提供的 `define_model_callbacks` 和 ActiveSupport 提供的 `define_callbacks` 方法，共生成十几个过滤器方法。

```ruby
# ActiveRecord::Callbacks
define_model_callbacks :initialize, :find, :touch, :only => :after
define_model_callbacks :save, :create, :update, :destroy

# ActiveModel::Callbacks
define_callbacks :validation

# ActiveRecord::Transactions
define_callbacks :commit, :rollback
```

和 AbstractController::Callbacks::ClassMethods 用元编程生成过滤器的方法名，是两种手法(尽管最终都是基于ActiveSupport::Callbacks)。

## 是什么？

通过钩子的方式，影响对象的生命周期。

## 有哪些？

共 19 个：

```ruby
CALLBACKS = [
  :after_initialize, :after_find, :after_touch,
  :before_save, :around_save, :after_save,
  :before_create, :around_create, :after_create,
  :before_update, :around_update, :after_update,
  :before_destroy, :around_destroy, :after_destroy,
  :before_validation, :after_validation, # => 需要定义
  :after_commit, :after_rollback # => 需要定义
]
```

## 怎么使用？

调用方式主要有以下几种:

1. 后面跟方法名，直接调用 √
2. 传递一个可回调对象 √
3. block 的形式，传递代码 √
4. 覆盖方法名，重新定义方法内容 √

1、3 用得最多，第 4 次之，还有一种方法不推荐(以字符串的形式传递)，第 2 可以起到分离和复用的作用，但复杂度提高了，并且有其它实现手法可替代。

```ruby
# 1
class Topic < ActiveRecord::Base
  before_destroy :delete_parents

  private
    def delete_parents
      self.class.delete_all "parent_id = #{id}"
    end
end
```

```ruby
# 2
class BankAccount < ActiveRecord::Base
  before_save      EncryptionWrapper.new
  after_save       EncryptionWrapper.new
  after_initialize EncryptionWrapper.new
end

class EncryptionWrapper
  def before_save(record)
    record.credit_card_number = encrypt(record.credit_card_number)
  end

  def after_save(record)
    record.credit_card_number = decrypt(record.credit_card_number)
  end

  alias_method :after_initialize, :after_save

  private
    def encrypt(value)
      # Secrecy is committed
    end

    def decrypt(value)
      # Secrecy is unveiled
    end
end
```

```ruby
# 3
class Napoleon < ActiveRecord::Base
  before_destroy { logger.info "Josephine..." }
  ...
end
```

```ruby
# 4
class Topic < ActiveRecord::Base
  def before_destroy() destroy_author end
end

class Reply < Topic
  def before_destroy() destroy_readers end
end
```

## 特殊情况，特殊处理。

## 怎么取消后面的回调？

返回 false

### prepend

```ruby
class Topic < ActiveRecord::Base
  has_many :children, dependent: destroy

  before_destroy :log_children

  private
    def log_children
      # Child processing
    end
end
```

已经被删除了，没办法 log_children，可以改为这样：

```ruby
class Topic < ActiveRecord::Base
  has_many :children, dependent: destroy

  before_destroy :log_children, prepend: true

  private
    def log_children
      # Child processing
    end
end
```
