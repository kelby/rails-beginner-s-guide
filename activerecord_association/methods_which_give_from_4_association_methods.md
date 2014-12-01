## 使用关联方法后，Rails 自动生成了哪些方法

### 一对一关系

| 生成的方法                 | belongs_to |  belongs_to :polymorphic | has_one|
|----------------------------------|:----------:|:------------:|:-------:|
|association(force_reload = false)                             |     √      |      √       |    √    |
|association=(associate)                     |     √      |      √       |    √    |
|build_association(attributes={})        |     √      |              |    √    |
|create_association(attributes={})       |     √      |              |    √    |
|create_association!(attributes={})      |     √      |              |    √    |

这些方法主要由 Association 和 Singular Association 提供。

### 一对多、多对多关系

| 生成的方法                 | habtm | has_many | has_many :through|
|----------------------------------|:-----:|:--------:|:--------:|
| collection(force_reload = false) | √ | √ | √ |
| collection<<(object, …) | √ | √ | √ |
| collection.delete(object, …) | √ | √ | √ |
| collection.destroy(object, …) | √ | √ | √ |
| collection=objects | √ | √ | √ |
| collection_singular_ids | √ | √ | √ |
| collection_singular_ids=ids | √ | √ | √ |
| collection.clear | √ | √ | √ |
| collection.empty? | √ | √ | √ |
| collection.size | √ | √ | √ |
| collection.find(…) | √ | √ | √ |
| collection.exists?(…) | √ | √ | √ |
| collection.build(attributes = {}, …) | √ | √ | √ |
| collection.create(attributes = {}) | √ | √ | √ |
| collection.create!(attributes = {}) | √ | √ | √ |

这些方法主要由 Association 和 Collection Association 提供。
还有一些 Collection Proxy 提供的方法，不在此列举。
