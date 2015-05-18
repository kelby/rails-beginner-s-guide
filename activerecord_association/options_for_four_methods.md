## 关联方法的可选参数汇总

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:class_name                           |   √   |    √     |    √  |  √ |
|:foreign_key                          |   √   |    √     |    √  |  √ |
|:validate                             |   √   |    √     |    √  |  √ |
|:autosave                             |   √   |    √     |    √  |  √ |
|:primary_key                          |   √   |    √     |    √  |    |
|:dependent                            |   √   |    √     |    √  |    |
|:inverse_of                           |   √   |    √     |    √  |    |
|:as                                   |   √   |    √     |       |    |
|:through                              |   √   |    √     |       |    |
|:source                               |   √   |    √     |       |    |
|:source_type                          |   √   |    √     |       |    |
|:required                             |   √   |          |    √  |    |
|:counter_cache                        |       |    √     |    √  |    |
|:polymorphic                          |       |          |    √  |    |
|:touch                                |       |          |    √  |    |
|:foreign_type                         |   √   |    √     |    √  |    |
|:join_table                           |       |          |       |  √ |
|:association_foreign_key              |       |          |       |  √ |
|:readonly                             |   V   |   V      |  V    |  √ |

`class_name` 指定关联对象所对应的"类/Model"，默认是"驼峰"转换关联对象的名字。

`foreign_key` 在自己的表里，用什么字段表示/存储关联对象的"外键"，默认是"关联对象_id"

`validate` 保存自己的时候，校验内存里的关联对象(不是相应字段)是否存在于数据库里。

1 设置了 validate<br>
2 不设置 validate，但设置了 autosave, 或 has_many 和 habtm 关联。<br>
3 方法名是：autosave_association 里的 :"validate_associated_records_for_#{关联对象}"<br>
4 内容是 :validate_collection_association 或 :validate_single_association<br>
5 后面使用 validate 进行执行<br>
6 校验关联是否成立，并且关联对象是否存在。

类似 validate_presence_of :关联对象

`autosave` 保存自己时，自动保存关联对象。

`primary_key` 查询关联对象的时候，用自己的哪个字段做为条件。

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
