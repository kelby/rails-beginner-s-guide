
## 3) has_many

### 方法本身表示什么意思。

指定一对多关系。

### 引进了哪些方法，表示什么意思。

| 方法 | 解释 |
| -- | -- |
| collection(force_reload = false) | 类似 attr_reader. 如果没有的话返回一个空的 Relation (类似空数组).|
| collection<<(object, …) | 设置后者的"前者_id"属性的值为"前者.id"，即可将后者与前者关联起来。只要前者非 new_record，此操作立即生效。一个前者可以关联多个后者，这里是追加操作，也就是说关联数量会增加。|
| collection.delete(object, …) | 设置后者的"前者_id"属性的值为 nil，即可解除前者与后者的关联，这里只是更新(不是删除)。设置可选参数 dependent: :destroy, 则后者会被 destroy; 设置可选参数 dependent: :delete_all, 则后者会被 delete. <br> 如果设置了可选参数 :through, 则关联表'默认'会被删除(不是更新)，这里用'默认'，说明可配置。|
| collection.destroy(object, …) | destroy 后者，这里优先级大于可选参数 :dependent，并且这里会触发回调。 <br> 如果设置了可选参数 :through, 则关联表'立即'会被删除(不是更新)，这里用'立即'，说明不可配置。|
| collection=objects | 设置后者与前者的关联。一个前者可以关联多个后者，这里包含增加、删除操作，也就是说关联数量可增加、减少或不变。此操作是"<<" 和 "delete"操作的结合版本。<br> 如果设置了可选参数 :through, 则关联表'立即'会被增加或删除(不是更新)。|
| collection_singular_ids | 返回所有关联后者的 id.|
| collection_singular_ids=ids | 类似 collection= 只不过这里传递的是 id.|
| collection.clear | 移除关联的后者，这里是移除关联，不是删除。**除非** 设置了 dependent: :destroy 则后者会被 destroy; 设置了 dependent: delete_all 则后者被被 delete. <br> 如果设置了可选参数 :through, 则关联表'立即'会被删除(不是更新)。|
| collection.empty? | 如果没有关联对象，返回 true |
| collection.size | 返回关联对象的数目 |
| collection.find(…) | 从关联对象里查找对象，和 ActiveRecord::Base.find 一样。|
| collection.exists?(…) | 检测某个关联对象是否存在。和 ActiveRecord::Base.exists? 一样。 |
| collection.build(attributes = {}, …) | 创建一个或多个关联对象，这里已经设置了"前者_id"属性的值，但还没有保存到数据库。 |
| collection.create(attributes = {}) | 创建一个或多个关联对象，这里已经设置了"前者_id"属性的值，并且已经保存到了数据库  (前提是通过校验)。<br> 使用这个方法，有个前提条件，前者不能是 new_record. |
| collection.create!(attributes = {}) | 类似 collection.create, 但保存失败的话会报错 ActiveRecord::RecordInvalid |

以上解释，参考了官方文档。但因为各个参数组合起来，呈几何倍数的增长，验证过程难免有疏漏，实际情况以运行结果为准。

### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
has_many :comments, -> { where(author_id: 1) }
has_many :employees, -> { joins(:address) }
has_many :posts, ->(post) { where("max_post_length > ?", post.length) }
```

普通参数 **Extension**

前面提到过，使用这几个关联方法时，"它们会引入一些额外的方法"，并且上文已经介绍了它们。但如果我们想要引入自己的方法，并且用途基本一样，要怎么做，可以用 Extension.

举例:

```ruby
class Organization < ActiveRecord::Base
  has_many :people do
    def find_active
      find(:all, :conditions => ["active = ?", true])
    end
  end
end
```

现在，可以用 `organization.people.find_active` 对后者进行操作。这样定义的方法，和 CollectionProxy 定义的方法类似。

> Note: 这些 Extension 方法，类似在后者 model 里定义的 scope, 但只能通过前者.后者这种方法调用！

可选参数 **Options**

| 参数 | 解释 |
| -- | -- |
| :class_name | 解释同上 |
| :foreign_key | 解释同上 |
| :primary_key | 解释同上 |
| :dependent | 可选 :destroy，也就是使用 destroy 删除所有关联对象；可选 :delete_all，也就是使用 delete 删除所有关联对象；可选 :nullify，把外键设为 nil，但不删除对象；可选 :restrict_with_exception，有关联对象则抛异常；可选 :restrict_with_error，有关联对象则抛错误 |
| :counter_cache | 定制用什么字段保存关联表的统计数目 |
| :as | 解释参考 belongs_to |
| :through | 指定中间表。优先级比 :class_name, :primary_key 和 :foreign_key 要高。<br> 如果对应的关联是 belongs_to 则，关联表会被自动更新、创建、删除。否则，关联表为只读状态，也就是说创建后你就只能手动维护，不会自动更新、删除。 <br> 如果你要更改关联，最好配置一下 :inverse_of 选项，以便被关联对象及时更新与其父亲的关系。 |
| :source | 配合 :through 使用，当查询关联表数据时用哪张表的字段。例如 has_many :subscribers, through: :subscriptions，如果不指定，默认会查询 :subscribers 或 :subscriber 表 |
| :source_type | 从后者的角度来看，后者与前者的关联应该是 belongs_to. 但如果恰好又是多态，那么后者保存有前者的 id 并指定某个类型. 如果你对按约定生成的类型不满意，可以用 :source_type 指明。 |
| :validate | 同上。但默认是 true |
| :autosave | 设置为 true，保存父亲对象时，其关联对象同时被保存。设置为 false, 对关联对象不做任何操作。默认，只有被关联对象为 new_record 时才会自动保存。<br> 使用 accepts_nested_attributes_for 会自动设置 :autosave 为 true. |
| :inverse_of | 解释同上 |

### 注意事项。

- :dependent 可选参数
  - :destroy 删除(destroy)所有被关联对象。

  - :delete_all 和 destroy 类似，也是删除所有被关联对象。但区别在于，此删除操作不会触发回调。

  - :nullify 设置后者的"前者_id"属性为 nil. 不会触发回调。

  - :restrict_with_exception 有关联对象则抛异常。并且后面与之的无关代码也不能再运行

  - :restrict_with_error 有关联对象则设置对象的 errors 信息。并且后面与之无关的代码还能运行
- :autosave 以 before_save 的形式来调用，所以会受到其它 before_save 回调方法的影响。

