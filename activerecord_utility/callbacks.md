## Callbacks 的使用

基于 Action Model 提供的 `define_model_callbacks` 和 Active Support 提供的 `define_callbacks` 方法，共生成十几个过滤器方法。

```ruby
# ActiveRecord::Callbacks
define_model_callbacks :initialize, :find, :touch, :only => :after
define_model_callbacks :save, :create, :update, :destroy

# ActiveModel::Callbacks
define_callbacks :validation

# ActiveRecord::Transactions
define_callbacks :commit, :rollback
```

和 AbstractController::Callbacks::ClassMethods 用元编程生成过滤器的方法名，是两种手法(尽管最终都是基于 ActiveSupport::Callbacks).

### 是什么？

通过钩子的方式，影响对象的生命周期。

### 有哪些？

共 19 个：

```ruby
CALLBACKS = [
  :after_initialize, :after_find, :after_touch,
  :before_save, :around_save, :after_save,
  :before_create, :around_create, :after_create,
  :before_update, :around_update, :after_update,
  :before_destroy, :around_destroy, :after_destroy,
  :before_validation, :after_validation,
  :after_commit, :after_rollback
]
```

### 怎么使用？

调用方式主要有以下几种:

1. 宏定义的方式，后面跟方法名进行调用
2. 传递一个可回调对象
3. 以类方法的形式，传递一个 block

1、3 用得最多，第 2 次之，还有一种方法不推荐(以字符串的形式传递)，第 2 可以起到分离和复用的作用，但复杂度提高了，并且有其它实现手法可替代。

```ruby
# 1 宏定义的方式，后面跟方法名进行调用
class Topic < ActiveRecord::Base
  before_destroy :delete_parents

  private
    def delete_parents
      self.class.delete_all "parent_id = #{id}"
    end
end
```

回调是针对单个 record 对象而言的。当传递给回调的参数是一个实例对象时，把当前 record 对象当做参数，传递并执行实例对象里和回调同名的方法。创建实例对象的时候，你也可以传递参数。

```ruby
# 2 传递一个可回调对象
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

# 2 传递一个可回调对象
class BankAccount < ActiveRecord::Base
  before_save      EncryptionWrapper.new("credit_card_number")
  after_save       EncryptionWrapper.new("credit_card_number")
  after_initialize EncryptionWrapper.new("credit_card_number")
end

class EncryptionWrapper
  def initialize(attribute)
    @attribute = attribute
  end

  def before_save(record)
    record.send("#{@attribute}=", encrypt(record.send("#{@attribute}")))
  end

  def after_save(record)
    record.send("#{@attribute}=", decrypt(record.send("#{@attribute}")))
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
# 3 以类方法的形式，传递一个 block
class Napoleon < ActiveRecord::Base
  before_destroy { logger.info "Josephine..." }
  before_destroy do
    # some code
  end
  ...
end
```

### 抽取封装回调方法

覆盖方法名，重新定义方法内容(注意：这里定义的是实例方法啊，内容不会被执行，其它类再继承才能执行!) √

```ruby
# 1
class Topic < ActiveRecord::Base
  def before_destroy() destroy_author end
end

class Reply < Topic
  def before_destroy() destroy_readers end
end

# 2
class PictureFileCallbacks
  def after_destroy(picture_file)
    if File.exist?(picture_file.filepath)
      File.delete(picture_file.filepath)
    end
  end
end

class PictureFile < ActiveRecord::Base
  after_destroy PictureFileCallbacks.new
end

# 3
class PictureFileCallbacks
  def self.after_destroy(picture_file)
    if File.exist?(picture_file.filepath)
      File.delete(picture_file.filepath)
    end
  end
end

class PictureFile < ActiveRecord::Base
  after_destroy PictureFileCallbacks
end
```

### 怎么取消后面的回调？

在方法里返回 `false`

### 如何理解 around

用 around_save 举例：

```ruby
def around_save
   # 类似 before save ...
   yield # 执行 save
   # 类似 after save ...
end
```
