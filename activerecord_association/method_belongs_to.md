## belongs_to

#### 方法本身表示什么意思。

指定一对一关系。如果，"自己 belongs_to 关联对象"，那么自己需要包括"关联对象的外键"。

#### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| association(force_reload = false) | 类似 attr_reader. 如果没有的话，返回 nil. |
| association=(associate) | 类似 attr_writer. |
| build_association(attributes = {}) | 构建并返回关联对象。这里关联对象还"未保存"在数据库。|
| create_association(attributes = {}) | 创建并返回关联对象。这里关联对象会被"保存"在数据库(如果校验没有问题的话) |
| create_association!(attributes = {}) | 和 create_association 类似, 但如果创建失败会报错 ActiveRecord::RecordInvalid. |

后面几个方法，作用类似，只是细节部分有所不同，使用时注意一下即可。

#### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
# 关联对象的 id = 2
belongs_to :user, -> { where(id: 2) }
# 同时 join 关联对象的 friends
belongs_to :user, -> { joins(:friends) }
# 关联对象的 game_level 必须大于 level.current
belongs_to :level, ->(level) { where("game_level > ?", level.current) }
```

#### 其它

使用 primary_key 前后对比：

```ruby
author belongs_to :book

author.book
# => SELECT `books`.* FROM `books` WHERE `books`.`id` = author.id LIMIT 1

author belongs_to :book, primary_key: :alias_book_id

author.book
# => SELECT `books`.* FROM `books` WHERE `books`.`alias_book_id` = author.id LIMIT 1
```
