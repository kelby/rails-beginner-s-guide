## Validations 校验

和 Active Model 里的校验实现原理类似，但有一点不同：这里校验的属性要是关联对象或要从数据库'读'数据。

**1) validates_associated(*attr_names)**

校验是否存在关联(关系)。可以同时校验多个关联(关系)

```ruby
class Book < ActiveRecord::Base
  has_many :pages
  belongs_to :library

  validates_associated :pages, :library
end
```

**2) validates_presence_of(*attr_names)**

校验(数据库里)是否存在着关联对象。

**3) validates_uniqueness_of(*attr_names)**

校验属性的值是否唯一。默认是对所有 record 进行校验，可以用 scope 或 conditions 指定约束条件。

```ruby
class Person < ActiveRecord::Base
  validates_uniqueness_of :user_name
end

# 加 scope 约束条件
# 同一 account 的 person, user_name 不能相同
# 不同 account 的 person, user_name 可以相同
class Person < ActiveRecord::Base
  validates_uniqueness_of :user_name, scope: :account_id
end

# 加 conditions 约束条件
# status 为 archived 的 article，title 不能相同
# status 为其它值的 article，title 可以相同
class Article < ActiveRecord::Base
  validates_uniqueness_of :title, conditions: -> { where.not(status: 'archived') }
end
```

同名实例方法：

```
save
save!

valid? & validate
validate!
```

这里的 `save` 是对 Persistence(持久化)里的 save 方法做的一层包装，在"保存"之前用来做校验工作，并不是真正的保存操作。当传递 `validate: false` 时，可以跳过此校验。其它同名实例方法意义类似。
