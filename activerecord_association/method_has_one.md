## 2) has_one

### 方法本身表示什么意思。

指定一对一关系。如果，"前者 has_one 后者"，那么'后者'需要包括'前者_id'属性。

### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| association(force_reload = false) | 类似 attr_reader. 如果没有的话，返回 nil. |
| association=(associate) | 类似 attr_writer. |
| build_association(attributes = {}) | 构建并返回后者。这里后者还未保存在数据库。 |
| create_association(attributes = {}) | 创建并返回后者。这里后者会被保存在数据库(如果校验没有问题的话) |
| create_association!(attributes = {}) | 和 create_association 类似, 但如果创建失败会报错 ActiveRecord::RecordInvalid. |

这里的解释参考了 `belongs_to`，不单它们的方法名是一样的。定们定义方法就也是一样的。

另，belongs_to 和 has_one 都属于 SingularAssociation.

### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
has_one :author, -> { where(comment_id: 1) }
has_one :employer, -> { joins(:company) }
has_one :dob, ->(dob) { where("Date.new(2000, 01, 01) > ?", dob) }
```

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 后者对应着的'类名'. 当按照"约定"找不到这个'类'的时候，用 :class_name 指明。 |
| :dependent | 删除前者时，对后者进行什么操作。1) :destroy, 删除后者，会触发回调；2) :delete, 删除后者，不会触发回调；3) ::nullify, 把后者里的"前者_id"属性设置为 nil, 不会触发回调；4) :restrict_with_exception, 如果有后者的关联对象，报异常；5) :restrict_with_error 如果有后者的关联对象，报错。|
| :foreign_key* | 前者的外键，默认是"前者_id"，这个属性是按照"约定"命名的，如果不符合"约定"，用 :foreign_key 指明。 |
| :primary_key* | 凡是通过前者查询后者。传递的是前者的主键所对应的值，也就是 id 字段的值，如果觉得不合适，可以用 :primary_key 指明字段。 |
| :as | 多态时用到，声明多态的接口。通常和 belongs_to 里的 :polymorphic 配对使用。|
| :through | 指定中间表。优先级比 :class_name、:primary_key 和 :foreign_key 要高。|
| :source | 中间表关联着前者和后者，并且"前者.后者"可拆分成1)"前者.中间表"，2)"中间表.后者"。第 1 步一般不会有误，但如果后者名字不规范，那么在第 2 步"中间表.后者"就会走不下去。用 :source 明确后者对应中间表里的什么关联。|
| :source_type | 从后者的角度来看，后者与前者的关联应该是 belongs_to. 但如果恰好又是多态，那么后者保存有前者的 id 并指定某个类型. 如果你对按约定生成的类型不满意，可以用 :source_type 指明。|
| :validate | 设置为 false, 保存前者时不会校验后者。默认就是 false.|
| :autosave | 参考 belongs_to 的解释 |
| :inverse_of | 参考 belongs_to 的解释 |
| :required | 参考 belongs_to 的解释 |

### 注意事项。

- `:foreign_key 和 primary_key` 针对的是前者。
- `:through` 实现关联时用到了 reflection 的代码，所以 has_one 或 belongs_to 关联要使用中间表，只能通过 :through 这一种方式，区别于 has_many :through 和 has_and_belongs_to_many.
- `:source_type` 影响的是数据，不是属性。

### 对比

使用 primary_key 前后对比

```ruby
book has_one :author

book.author
# 传递的值是 book.id
# => Author Load (6.5ms)  SELECT  `authors`.* FROM `authors` WHERE `authors`.`book_id` = book.id LIMIT 1

book has_one :author, primary_key: :a_primary_id

book.author
# 传递的值是 book.a_primary_id
# => Author Load (6.5ms)  SELECT  `authors`.* FROM `authors` WHERE `authors`.`book_id` = book.a_primary_id LIMIT 1
```
