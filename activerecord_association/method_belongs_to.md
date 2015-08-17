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

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 指定关联对象的"类名"。约定是："驼峰"转换关联对象的名字。不符合约定时，用 :class_name 指明。|
| :foreign_key | 指定关联对象的"外键"，保存在自己的表里。关联对象的"外键"是按照约定命名的，不符合约定时，用 :foreign_key 指明。|
| :foreign_type | 指定用什么"字段"保存关联对象的"多态信息"，保存在自己的表里，多态时用到。默认用"关联对象_type"这个字段，不符合约定时，用 :foreign_type 指明。|
| :primary_key | 指定关联对象的"主键"，保存在关联对象的表里，查询关联对象时用到。查询关联对象时，自己保存的外键对应着关联对象的主键(默认是 id)，如果不符合约定，可以用 :primary_key 指明。|
| :dependent | 如果设置为 :destroy, 自己被 destroy 时，关联对象会被 destroy. 如果设置为 :delete, 自己被 destroy 时，关联对象会被 delete. 注意：自己被 delete, 始终不影响关联对象。|
| :counter_cache | 设置为 true 后，自己被创建或删除，会改变关联对象里"计数器的值"。默认是 false，也就是不起作用。|
| :polymorphic | 声明此关联是多态的。|
| :validate | 设置为 true, 保存自己的时候，会先校验它的关联对象。默认是 false, 也就是不校验。|
| :autosave | 设置为 true, 保存自己的时候，同时保存它的关联对象(用的是 before_save)。设置为 false 还可分为两种情况：前者为 new_record，则保存自己时会自动保存关联对象；否则上述自动操作都不会被执行。|
| :touch | 设置为 true, 保存或 destroy 自己的时候，关联对象的 updated_at/on 属性会被更新。|
| :inverse_of | 保证 object_id 相同。通过前者(1)查询到后者，然后再通过后者返过来查询前者(2)。按照直观的理解，(1) 和 (2) 应该是同样的对象，同样的值。但实际情况会发现，它们不一样(可以通过 object_id 确定)！原因是程序没有这么聪明，没法判断它们是一样的(特别是通过中间表查询时)。设置 :inverse_of，可解决这个问题。|
| :required | 语法糖。原来的做法是 "belongs_to 关联对象" + "validates_presence_of 关联对象"，现改为 "belongs_to 关联对象, required: true"|

#### 注意事项。

- 自己包含了"关联对象_id"属性，注意和下文提到的 has_one 的区别。
- `:class_name` 不影响 `:foreign_key` "关联对象_id"属性的命名约定。
- 当关联对象与自己的关系是 has_many 时，请慎用 `:dependent`. 因为关联对象被同时删除的话，意味着自己的兄弟将成为孤儿(没有关联对象可关联)。
- `:counter_cache` 计数器是根据"前者对应的 table_name + count"生成的，如里不符合"约定"，可以用 :counter_cache 指明。自定义计数器名字的时候，建议把此属性声明为 attr_readonly. 综上，:counter_cache 可以设置为 true、false、或自定义的名字。
- `:polymorphic` 如果你同时使用了 `:counter_cache`，建议在后者的 model 里把计数器设置为 attr_readonly.
- 综上两条注意事项，有两种情况建议在后者的 model 里手动把计数器设置为 attr_readonly.
- `:validate` 对于是否是 new_record 在做 valid? 时，会造成迷惑。牢记，`:validate` 意味着对前者和后者都有"保存"操作。如果校验失败，则本次保存失败。
- `:autosave` 如果在 model 里使用了 accepts_nested_attributes_for，则对应 `:autosave` 始终为 true.
- `:touch` 如果你不是设置成 true, 而是传递一个符号 :symbol，那么这个符号会被更新为当前时间。
- `:inverse_of` 带来额外的好处：1. 不用重复查询，节省了性能；2. 因为对象的 object_id 都是一样的，保证了数据一致性；3. 注意顺序前者查询后者，然后后者反向查询前者；需要注意：第 2 次"查询"不是数据库查询，会存在操作脏数据的风险。
- `:inverse_of` 通常情况下，不用设置，会自动转换。但使用了以下参数，则不会自动转换：:through、:as、:polymorphic 和 :conditions；遇到单复数不规则，有时候也不会自动转换
- `:required` "validates_presence_of 关联对象" 和 "validates_presence_of 关联对象_id" 大同小异。前者是真实存在的对象，后者只是字段。

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
