#### 关联方法的可选参数汇总

**实现关联对象：**

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:class_name                           |   √   |    √     |    √  |  √ |
|:foreign_key                          |   √   |    √     |    √  |  √ |
|:primary_key                          |   √   |    √     |    √  |    |
|:as                                   |   √   |    √     |       |    |
|:through                              |   √   |    √     |       |    |
|:source                               |   √   |    √     |       |    |
|:source_type                          |   √   |    √     |       |    |
|:foreign_type                         |   √   |    √     |    √  |    |
|:join_table                           |       |          |       |  √ |
|:association_foreign_key              |       |          |       |  √ |

**和关联有关的回调(删除、自动保存等)：**

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:autosave                             |   √   |    √     |    √  |  √ |
|:dependent                            |   √   |    √     |    √  |    |
| after_add                            |       |          |       |  √  |

**和关联有关的校验：**

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:validate                             |   √   |    √     |    √  |  √ |
|:required                             |   √   |          |    √  |    |

**其它：**

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:counter_cache                        |       |    √     |    √  |    |
|:polymorphic                          |       |          |    √  |    |
|:touch                                |       |          |    √  |    |
|:readonly                             |   V   |   V      |  V    |  √ |
|:inverse_of                           |   √   |    √     |    √  |    |

class_name

```
指定关联对象所对应的"类/Model"，默认是"驼峰"转换关联对象的名字。
不符合约定时，用 :class_name 指明。
:class_name 不影响 :foreign_key "关联对象_id"属性的命名约定。
```

foreign_key

```
在自己的表里，用什么字段表示/存储关联对象的"外键"，默认是"关联对象_id"
:foreign_key 和 primary_key 针对的都是前者。
```

primary_key

```
查询关联对象的时候，用自己的哪个字段做为条件。
指定关联对象的"主键"，保存在关联对象的表里，查询关联对象时用到。查询关联对象时，自己保存的外键对应着关联对象的主键(默认是 id)，如果不符合约定，可以用 :primary_key 指明。
```

`validate` 保存自己的时候，校验内存里的关联对象(不是相应字段)是否存在于数据库里。

1 设置了 validate<br>
2 不设置 validate，但设置了 autosave, 或 has_many 和 habtm 关联。<br>
3 方法名是：autosave_association 里的 :"validate_associated_records_for_#{关联对象}"<br>
4 内容是 :validate_collection_association 或 :validate_single_association<br>
5 后面使用 validate 进行执行<br>
6 校验关联是否成立，并且关联对象是否存在。

类似 validate_presence_of :关联对象

`autosave` 保存自己时，自动保存关联对象。



`dependent` 分为几种情况：

1 belongs_to 只有 destroy 和 delete<br>
2 has_many 有 destroy、delete_all、nullify、restrict_with_exception 和 restrict_with_error<br>
3 has_one  有 destroy、delete、nullify、restrict_with_exception 和 restrict_with_error

`inverse_of` 通过自己查找到关联对象，然后又通过关联对象找回自己。

有的关联会自动推断 inverse_of，所以用不用其实效果一样。而有的关联加上参数后，不起作用，所以即使设置也没用。可以根据以下代码进行检测：

```ruby
ModelName.reflections.map do |key, value|
  p "#{key} inverse_of: #{value.has_inverse?}"
end
```

`as` 多态时，自己是什么接口。

`through` 在中间表里，希望关联对象怎么被表示。(听起来是不是有点绕，管得也太多了吧)

`source` 必须和 through 配合使用，自己在中间表里用什么表示。(类似 :as)

`source_type` 必须和 polymorphic 配合使用。多态时希望自己用什么做为类型。

`counter_cache` 分为几种情况：

1 belongs_to 里设置为 true 表明使用 counter. (字段使用默认)<br>
2 belongs_to 里设置 counter 字段。<br>
3 has_many   里设置 counter 字段。

`polymorphic` 多态。

`touch` 

1 设置为 true，更新自己后，更新关联对象的 updated_at/on 字段。<br>
2 设置为其它字段，则更新自己后，额外更新关联对象的 updated_at/on 和其它字段。

`foreign_type`

1 对于 has_one 和 has_many，用什么字段做为自己的外键。<br>
2 对于 belongs_to，用什么字段做为关联对象的外键。

`join_table` 中间表。

`required` 要求有关联对象存在于数据库。

`association_foreign_key` 在中间表里，希望关联对象用什么字段做为外键。(听起来是不是有点绕，管得也太多了吧)

`readonly` 所有关联对象为只读。

#### belongs_to

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :foreign_type | 指定用什么"字段"保存关联对象的"多态信息"，保存在自己的表里，多态时用到。默认用"关联对象_type"这个字段，不符合约定时，用 :foreign_type 指明。|
| :dependent | 如果设置为 :destroy, 自己被 destroy 时，关联对象会被 destroy. 如果设置为 :delete, 自己被 destroy 时，关联对象会被 delete. 注意：自己被 delete, 始终不影响关联对象。|
| :counter_cache | 设置为 true 后，自己被创建或删除，会改变关联对象里"计数器的值"。默认是 false，也就是不起作用。|
| :polymorphic | 声明此关联是多态的。|
| :validate | 设置为 true, 保存自己的时候，会先校验它的关联对象。默认是 false, 也就是不校验。|
| :autosave | 设置为 true, 保存自己的时候，同时保存它的关联对象(用的是 before_save)。设置为 false 还可分为两种情况：前者为 new_record，则保存自己时会自动保存关联对象；否则上述自动操作都不会被执行。|
| :touch | 设置为 true, 保存或 destroy 自己的时候，关联对象的 updated_at/on 属性会被更新。|
| :inverse_of | 保证 object_id 相同。通过前者(1)查询到后者，然后再通过后者返过来查询前者(2)。按照直观的理解，(1) 和 (2) 应该是同样的对象，同样的值。但实际情况会发现，它们不一样(可以通过 object_id 确定)！原因是程序没有这么聪明，没法判断它们是一样的(特别是通过中间表查询时)。设置 :inverse_of，可解决这个问题。|
| :required | 语法糖。原来的做法是 "belongs_to 关联对象" + "validates_presence_of 关联对象"，现改为 "belongs_to 关联对象, required: true"|

**注意事项：**

- 自己包含了"关联对象_id"属性，注意和下文提到的 has_one 的区别。
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

#### has_one

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 后者对应着的'类名'. 当按照"约定"找不到这个'类'的时候，用 :class_name 指明。 |
| :dependent | 删除前者时，对后者进行什么操作。1) :destroy, 删除后者，会触发回调；2) :delete, 删除后者，不会触发回调；3) ::nullify, 把后者里的"前者_id"属性设置为 nil, 不会触发回调；4) :restrict_with_exception, 如果有后者的关联对象，报异常；5) :restrict_with_error 如果有后者的关联对象，报错。|
| :foreign_type | 多态时，在关联对象的表里，用什么字段来存储父亲对象的类型(默认是 x_type，根据 :as 而来) |
| :primary_key* | 凡是通过前者查询后者。传递的是前者的主键所对应的值，也就是 id 字段的值，如果觉得不合适，可以用 :primary_key 指明字段。 |
| :as | 多态时用到，声明多态的接口。通常和 belongs_to 里的 :polymorphic 配对使用。|
| :through | 指定中间表。优先级比 :class_name、:primary_key 和 :foreign_key 要高。|
| :source | 中间表关联着前者和后者，并且"前者.后者"可拆分成1)"前者.中间表"，2)"中间表.后者"。第 1 步一般不会有误，但如果后者名字不规范，那么在第 2 步"中间表.后者"就会走不下去。用 :source 明确后者对应中间表里的什么关联。|
| :source_type | 从后者的角度来看，后者与前者的关联应该是 belongs_to. 但如果恰好又是多态，那么后者保存有前者的 id 并指定某个类型. 如果你对按约定生成的类型不满意，可以用 :source_type 指明。|
| :validate | 校验关联对象是否真实存在于数据库里。设置为 false, 保存前者时不会校验后者。默认就是 false.|
| :autosave | 参考 belongs_to 的解释 |
| :inverse_of | 参考 belongs_to 的解释 |
| :required | 参考 belongs_to 的解释 |

**注意事项：**

- `:through` 实现关联时用到了 reflection 的代码，所以 has_one 或 belongs_to 关联要使用中间表，只能通过 :through 这一种方式，区别于 has_many :through 和 has_and_belongs_to_many.
- `:source_type` 影响的是数据，不是属性。
- `:foreign_type` 没有这个选项之前，这个字段只能根据 `:as` 生成，不能自定义。

#### has_many

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 解释同上。关联对象不符合约定 |
| :foreign_type | 自己不符合约定。多态时，在关联对象的表里，用什么字段来存储父亲对象的类型(默认是 x_type，根据 :as 而来) |
| :primary_key | 解释同上。自己不符合约定 |
| :dependent | 可选 :destroy，也就是使用 destroy 删除所有关联对象；可选 :delete_all，也就是使用 delete 删除所有关联对象；可选 :nullify，把外键设为 nil，但不删除对象；可选 :restrict_with_exception，有关联对象则抛异常；可选 :restrict_with_error，有关联对象则抛错误 |
| :counter_cache | 定制用什么字段保存关联表的统计数目 |
| :as | 解释参考 belongs_to |
| :through | 指定中间表。优先级比 :class_name, :primary_key 和 :foreign_key 要高。<br> 如果对应的关联是 belongs_to 则，关联表会被自动更新、创建、删除。否则，关联表为只读状态，也就是说创建后你就只能手动维护，不会自动更新、删除。 <br> 如果你要更改关联，最好配置一下 :inverse_of 选项，以便被关联对象及时更新与其父亲的关系。 |
| :source | 配合 :through 使用，当查询关联表数据时用哪张表的字段。例如 has_many :subscribers, through: :subscriptions，如果不指定，默认会查询 :subscribers 或 :subscriber 表 |
| :source_type | 从后者的角度来看，后者与前者的关联应该是 belongs_to. 但如果恰好又是多态，那么后者保存有前者的 id 并指定某个类型. 如果你对按约定生成的类型不满意，可以用 :source_type 指明。 |
| :validate | 同上。但默认是 true |
| :autosave | 设置为 true，保存父亲对象时，其关联对象同时被保存。设置为 false, 对关联对象不做任何操作。默认，只有被关联对象为 new_record 时才会自动保存。<br> 使用 accepts_nested_attributes_for 会自动设置 :autosave 为 true. |
| :inverse_of | 解释同上 |

**注意事项：**

- `:dependent` 可选参数
  - `:destroy` 删除(destroy)所有被关联对象。

  - `:delete_all` 和 destroy 类似，也是删除所有被关联对象。但区别在于，此删除操作不会触发回调。

  - `:nullify` 设置后者的"前者_id"属性为 nil. 不会触发回调。

  - `:restrict_with_exception` 有关联对象则抛异常。并且后面与之的无关代码也不能再运行。

  - `:restrict_with_error` 有关联对象则设置对象的 errors 信息。并且后面与之无关的代码还能运行。
- `:autosave` 以 before_save 的形式来调用，所以会受到其它 before_save 回调方法的影响。
- `:foreign_type` 没有这个选项之前，这个字段只能根据 `:as` 生成，不能自定义。

#### has_and_belongs_to_many

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 同上 |
| :join_table | 指定中间表的名字 **注意:** 如果你给中间表加了对应的'类'，并且命名不符合约定的话。那么一定要记得在每个 has_and_belongs_to_many 的地方都要设置 join_table |
| :association_foreign_key | 指定后者所对应的外键，如 Person has_and_belongs_to_many :projects, 则中间表里后者所对应的外键是 “project_id”。如果不符合要求，你使用使用 :association_foreign_key 设置 |
| :readonly | 设置为 true, 通过前者查询到的后者限制为只读状态，不可更改。但其它方式查询出来的，不受此限制。|
| :validate | 解释同上。但默认为 true |
| :autosave | 解释同上 |

**注意事项：**

- 注意前者与后者的顺序比较方法，如果前几个字符一样，则往后一个个字符比较，如 "paper_boxes" 和 "papers" 生成的中间表名字是 "paper_boxes_papers" 而不是 "papers_paper_boxes". 
- 你可以用可选参数 :join_table 指定中间表名字。
- 如果前者和后者使用的表名都带有前缀并且还相同，那么中间表的名字也用同样的前缀，剩余部分用再按字母顺序排序。如："catalog_categories" 和 "catalog_products" 生成的中间表是 "catalog_categories_products".
