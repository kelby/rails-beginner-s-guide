# Callbacks

## 是什么？

Callbacks are hooks into the life cycle of an Active Record object that allow you to trigger logic before or after an alteration of the object state.

## 有哪些？

共 19 个：

```ruby
CALLBACKS = [
  :after_initialize, :after_find, :after_touch, :before_validation, :after_validation,
  :before_save, :around_save, :after_save, :before_create, :around_create,
  :after_create, :before_update, :around_update, :after_update,
  :before_destroy, :around_destroy, :after_destroy, :after_commit, :after_rollback
]
```

## 怎么使用？

There are four types of callbacks accepted by the callback macros:

1. Method references (symbol), √
2. callback objects, √
3. inline methods (using a proc), √
4. and inline eval methods (using a string). X
5. regular overwritable methods √

Method references and callback objects are the recommended approaches,
inline methods using a proc are sometimes appropriate (such as for creating mix-ins),
and inline eval methods are deprecated.
when you want to leave it up to each descendant to decide whether they want to call super and trigger the inherited callbacks.

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
  before_destroy 'self.class.delete_all "parent_id = #{id}"'
end
```

```ruby
# 5
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
