### 使用关联方法后，Rails 自动生成了哪些方法

**一对一关系**

| 生成的方法                 | belongs_to |  belongs_to :polymorphic | has_one|
|----------------------------------|:----------:|:------------:|:-------:|
|association(force_reload = false)                             |     √      |      √       |    √    |
|association=(associate)                     |     √      |      √       |    √    |
|build_association(attributes={})        |     √      |              |    √    |
|create_association(attributes={})       |     √      |              |    √    |
|create_association!(attributes={})      |     √      |              |    √    |

这些方法主要由 Association 和 Singular Association 提供。

**一对多、多对多关系**

最基本的：

| 生成的方法                 | habtm | has_many | has_many :through|
|----------------------------------|:-----:|:--------:|:--------:|
| collection(force_reload = false) | √ | √ | √ |
| collection=objects | √ | √ | √ |
| collection_singular_ids | √ | √ | √ |
| collection_singular_ids=ids | √ | √ | √ |

这些方法主要由 Association 和 Collection Association 提供。

高阶一点的：

| 生成的方法                 | habtm | has_many | has_many :through|
|----------------------------------|:-----:|:--------:|:--------:|
| collection<<(object, …) | √ | √ | √ |
| collection.delete(object, …) | √ | √ | √ |
| collection.destroy(object, …) | √ | √ | √ |
| collection.clear | √ | √ | √ |
| collection.empty? | √ | √ | √ |
| collection.size | √ | √ | √ |
| collection.find(…) | √ | √ | √ |
| collection.exists?(…) | √ | √ | √ |
| collection.build(attributes = {}, …) | √ | √ | √ |
| collection.create(attributes = {}) | √ | √ | √ |
| collection.create!(attributes = {}) | √ | √ | √ |

上述方法都是 Collection Proxy 提供的。(除上述方法外，还有的未列出)
