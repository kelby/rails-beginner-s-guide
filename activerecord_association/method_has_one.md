## has_one

#### 方法本身表示什么意思。

指定一对一关系。如果，"前者 has_one 后者"，那么'后者'需要包括'前者_id'属性。

#### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| association(force_reload = false) | 类似 attr_reader. 如果没有的话，返回 nil. |
| association=(associate) | 类似 attr_writer. |
| build_association(attributes = {}) | 构建并返回后者。这里后者还未保存在数据库。 |
| create_association(attributes = {}) | 创建并返回后者。这里后者会被保存在数据库(如果校验没有问题的话) |
| create_association!(attributes = {}) | 和 create_association 类似, 但如果创建失败会报错 ActiveRecord::RecordInvalid. |

这里的解释参考了 `belongs_to`，不单它们的方法名是一样的。定们定义方法就也是一样的。

另，belongs_to 和 has_one 都属于 Singular Association.

#### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
has_one :author, -> { where(comment_id: 1) }
has_one :employer, -> { joins(:company) }
has_one :dob, ->(dob) { where("Date.new(2000, 01, 01) > ?", dob) }
```

#### 其它

使用 primary_key 前后对比：

```ruby
book has_one :author

book.author
# 传递的值是 book.id
# => Author Load (6.5ms) SELECT `authors`.* FROM `authors` \n
                         WHERE `authors`.`book_id` = book.id LIMIT 1

book has_one :author, primary_key: :a_primary_id

book.author
# 传递的值是 book.a_primary_id
# => Author Load (6.5ms) SELECT `authors`.* FROM `authors` \n
                         WHERE `authors`.`book_id` = book.a_primary_id LIMIT 1
```
