## belongs_to

### 方法本身表示什么意思。

指定一对一关系。如果，"前者 belongs_to 后者"，那么'前者'需要包括'后者_id'属性。

### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| association(force_reload = false) | 类似 attr_reader. 如果没有的话，返回 nil. |
| association=(associate) | 类似 attr_writer. |
| build_association(attributes = {}) | 构建并返回后者。这里后者还未保存在数据库。|
| create_association(attributes = {}) | 创建并返回后者。这里后者会被保存在数据库(如果校验没有问题的话) |
| create_association!(attributes = {}) | 和 create_association 类似, 但如果创建失败会报错 ActiveRecord::RecordInvalid. |

后面几个方法，作用类似，只是细节部分有所不同，使用时注意一下即可。

### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
belongs_to :user, -> { where(id: 2) }
belongs_to :user, -> { joins(:friends) }
belongs_to :level, ->(level) { where("game_level > ?", level.current) }
```

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 后者对应着的'类名'. 当按照"约定"找不到这个'类'的时候，用 :class_name 指明。|
| :foreign_key | 后者的外键，这个外键是按照"约定"命名的，如果不符合"约定"，用 :foreign_key 指明。|
| :foreign_type | 多态时，后者所对应的类型，默认和'类名'一样，按照约定生成。当"后者_type"不符合"约定"时，用 :foreign_type 指明。|
| :primary_key | 凡是通过前者查询后者"。默认传递值给后者的主键(也就是 id 属性)，如果觉得不合适，可以用 :primary_key 指明。|
| :dependent | 如果设置为 :destroy, 前者被删除时，后者也会同时被 destroy. 如果设置为 :delete, 前者被删除时，后者也会同时被 delete. |
| :counter_cache | 设置为 true 后，创建或删除一个前者，会相应的调用 increment_counter 或 decrement_counter 来改变后者计数器的值。默认是 false，也就是不起作用。|
| :polymorphic | 声明 belongs_to 关联是多态的。|
| :validate | 设置为 true, 保存后者的时候会先对所有前者进行校验。默认是 false, 也就是不起作用。|
| :autosave | 设置为 true, 保存后者时会自动保存所有前者，destroy 后者时会自动删除所有前者。设置为 false 还可分为两种情况：前者为 new_record，则保存后者时会自动保存前者；否则上述自动操作都不会被执行。|
| :touch | 设置为 true, 保存或 destroy 前者的时候，后者的 updated_at/on 属性会被设置成当前时间。|
| :inverse_of | 保证 object_id 相同。通过前者(1)查询到后者，然后再通过后者返过来查询前者(2)。按照直观的理解，(1) 和 (2) 应该是同样的对象，同样的值。但实际情况会发现，它们不一样(可以通过 object_id 确定)！原因是程序没有这么聪明，没法判断它们是一样的(特别是通过中间表查询时)。设置 :inverse_of，可解决这个问题。|
| :required | 语法糖。原来的做法是 "belongs_to 后者" + "validates_presence_of 后者"，现改为 "belongs_to 后者, required: true"|

### 注意事项。

- 前者包含了"后者_id"属性，注意和下文提到的 has_one 的区别。
- `:class_name` 不影响 `:foreign_key` "后者_id"属性的命名约定。
- 当后者与前者的关系是 has_many 时，请慎用 `:dependent`. 因为后者被同时删除的话，剩下的前者将成为孤儿(没有前者可关联)。
- `:counter_cache` 计数器是根据"前者对应的 table_name + count"生成的，如里不符合"约定"，可以用 :counter_cache 指明。自定义计数器名字的时候，建议把此属性声明为 attr_readonly. 综上，:counter_cache 可以设置为 true、false、或自定义的名字。
- `:polymorphic` 如果你同时使用了 `:counter_cache`，建议在后者的 model 里把计数器设置为 attr_readonly.
- 综上两条注意事项，有两种情况建议在后者的 model 里手动把计数器设置为 attr_readonly.
- `:validate` 对于是否是 new_record 在做 valid? 时，会造成迷惑。牢记，`:validate` 意味着对前者和后者都有"保存"操作。如果校验失败，则本次保存失败。
- `:autosave` 如果在 model 里使用了 accepts_nested_attributes_for，则对应 `:autosave` 始终为 true.
- `:touch` 如果你不是设置成 true, 而是传递一个符号 :symbol，那么这个符号会被更新为当前时间。
- `:inverse_of` 带来额外的好处：1. 不用重复查询，节省了性能；2. 因为对象的 object_id 都是一样的，保证了数据一致性；3. 注意顺序前者查询后者，然后后者反向查询前者；需要注意：第 2 次"查询"不是数据库查询，会存在操作脏数据的风险。
- `:inverse_of` 通常情况下，不用设置，会自动转换。但使用了以下参数，则不会自动转换：:through、:as、:polymorphic 和 :conditions；遇到单复数不规则，有时候也不会自动转换
- `:required` "validates_presence_of 后者" 和 "validates_presence_of 后者_id" 大同小异。

### 对比

使用 primary_key 前后对比：

```ruby
author belongs_to :book

author.book
# => SELECT `books`.* FROM `books` WHERE `books`.`id` = author.id LIMIT 1

author belongs_to :book, primary_key: :alias_book_id

author.book
# => SELECT `books`.* FROM `books` WHERE `books`.`alias_book_id` = author.id LIMIT 1
```
