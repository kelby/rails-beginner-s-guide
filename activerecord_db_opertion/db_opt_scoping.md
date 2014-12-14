## Scoping

虽然只有 4 个方法，但很实用。

| 方法 | 解释 |
| -- | -- |
| scope | 命名 scope |
| default_scope | 设置默认 scope |
| unscoped | 跳过之前设置的 scope |
| all | all 方法，默认已经 scope |

### `scope(name, body, &block)`

重点说说这个方法。

两个参数：第一个是名字，第二个是内容，需要以 proc 的形式定义。

**scope 相当于类方法，可检索、查询对象**

可执行一系列的查询语句，如：where(color: :red).select('shirts.*').includes(:washing_instructions)

```ruby
class Shirt < ActiveRecord::Base
  scope :red, -> { where(color: 'red') }
  scope :dry_clean_only, -> { joins(:washing_instructions)
                              .where('washing_instructions.dry_clean_only = ?', true) }
end
```

可以调用 Shirt.red 和 Shirt.dry_clean_only. Shirt.red 功能和 Shirt.where(color: 'red') 一样。

也就是说，功能上和以下代码一样:

```ruby
class Shirt < ActiveRecord::Base
  def self.red
    where(color: 'red')
  end
end
```

不同的是 scope 返回的是 Relation，而不是数组. 所以，你可以调用 Shirt.red.first, Shirt.red.count, Shirt.red.where(size: 'small') 等. 但是 Relation 也可以有数组的行为, 如 Shirt.red.each(&block), Shirt.red.first, 和 Shirt.red.inject(memo, &block) 等。

**scioe 可以链式调用**

Shirt.red.dry_clean_only 运行结果是 red 和 dry_clean_only 综合的结果。Shirt.red.dry_clean_only.count 返回的是 red 和 dry_clean_only 综合结果的数目。在这里和 Shirt.red.dry_clean_only.average(:thread_count) 类似。

**scope 是是一步步走下去的**

```ruby
class Person < ActiveRecord::Base
  has_many :shirts
end
```

假设，elton 是 Person 的实例对象，则 elton.shirts.red.dry_clean_only 返回的是 elton(限制条件) 的 red 和 dry_clean_only shirts.

**scope 后可直接跟 extensions**

和 has_many 类似的：

```ruby
class Shirt < ActiveRecord::Base
  scope :red, -> { where(color: 'red') } do
    def dom_id
      'red_shirts'
    end
  end
end
```

**scope 后可直接跟 creating/building 等方法**

用于创建 record

```ruby
class Article < ActiveRecord::Base
  scope :published, -> { where(published: true) }
end

Article.published.new.published    # => true
Article.published.create.published # => true
```

**scope 后可直接跟类方法**

定义如下：

```ruby
class Article < ActiveRecord::Base
  scope :published, -> { where(published: true) }
  scope :featured, -> { where(featured: true) }

  def self.latest_article
    order('published_at desc').first
  end

  def self.titles
    pluck(:title)
  end
end
```

调用如下:

```ruby
Article.published.featured.latest_article
Article.featured.titles
```

### `default_scope`

1 一个参数，需要以 proc 的形式定义

```ruby
class Article < ActiveRecord::Base
  default_scope { where(published: true) }
end
```
2 始终起作用，不能覆盖，冲突时取合集。

3 会影响 initialization 过程

4 基于第2、3点请慎用

---

### 为什么参数要是 proc 类型？

```ruby
scope :recent, -> { where("created_at > ?", 2.day.ago) } 
```

可以看到这里的 2.day.ago 是动态生成的，每一次执行的时候才知道结果。不使用 proc 类型的话，这里立即执行，就和想像中的结果不同了。

> Note: 并不是所有的方法都可以做为 scope 的内容，更多内容 [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html#retrieving-objects-from-the-database)
