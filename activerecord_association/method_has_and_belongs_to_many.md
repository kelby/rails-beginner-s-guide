## has_and_belongs_to_many

#### 方法本身表示什么意思。

指定多对多关系。用中间表来连接前者与后者。如果没有明确的指定中间表的名字，那么默认按前者、后者的字母顺序排序构成中间表。

#### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| collection(force_reload = false) | 解释同上 |
| collection<<(object, …) | 解释同上 |
| collection.delete(object, …) | 解释同上 |
| collection.destroy(object, …) | 解释同上。但不会删除对象，只会删除连接表。 |
| collection=objects | 解释同上 |
| collection_singular_ids | 解释同上 |
| collection_singular_ids=ids | 解释同上 |
| collection.clear | 移除关联，也就是中间表数据。并没有删除后者。|
| collection.empty? | 解释同上 |
| collection.size | 解释同上 |
| collection.find(id) | 解释同上 |
| collection.exists?(…) | 解释同上 |
| collection.build(attributes = {}) | 解释同上 |
| collection.create(attributes = {}) | 解释同上 |

#### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
has_and_belongs_to_many :projects, -> { includes :milestones, :manager }
has_and_belongs_to_many :categories, ->(category) {
  where("default_category = ?", category.name)
}
```

普通参数 **Extension**

前面提到过，使用这几个关联方法时，"它们会引入一些额外的方法"，并且上文已经介绍了它们。但如果我们想要引入自己的方法，并且用途基本一样，要怎么做，可以用 Extension.

举例:

```ruby
has_and_belongs_to_many :contractors do
  def find_or_create_by_name(name)
    first_name, last_name = name.split(" ", 2)
    find_or_create_by(first_name: first_name, last_name: last_name)
  end
end
```

现在，可以用 `organization.people.find_active` 对后者进行操作。这样定义的方法，和 CollectionProxy 定义的方法类似。

> Note: 这些 Extension 方法，类似在后者 model 里定义的 scope, 但只能通过前者.后者这种方法调用！

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 同上 |
| :join_table | 指定中间表的名字 **注意:** 如果你给中间表加了对应的'类'，并且命名不符合约定的话。那么一定要记得在每个 has_and_belongs_to_many 的地方都要设置 join_table |
| :foreign_key | 同上 |
| :association_foreign_key | 指定后者所对应的外键，如 Person has_and_belongs_to_many :projects, 则中间表里后者所对应的外键是 “project_id”。如果不符合要求，你使用使用 :association_foreign_key 设置 |
| :readonly | 设置为 true, 通过前者查询到的后者限制为只读状态，不可更改。但其它方式查询出来的，不受此限制。|
| :validate | 解释同上。但默认为 true |
| :autosave | 解释同上 |

#### 注意事项。

- 注意前者与后者的顺序比较方法，如果前几个字符一样，则往后一个个字符比较，如 "paper_boxes" 和 "papers" 生成的中间表名字是 "paper_boxes_papers" 而不是 "papers_paper_boxes". 
- 你可以用可选参数 :join_table 指定中间表名字。
- 如果前者和后者使用的表名都带有前缀并且还相同，那么中间表的名字也用同样的前缀，剩余部分用再按字母顺序排序。如："catalog_categories" 和 "catalog_products" 生成的中间表是 "catalog_categories_products".
