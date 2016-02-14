## has_many

#### 方法本身表示什么意思。

指定一对多关系。

#### 引进了哪些方法，表示什么意思。

...

以上解释，参考了官方文档。但因为各个参数组合起来，呈几何倍数的增长，验证过程难免有疏漏，实际情况以运行结果为准。

### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
# scope 里面的查询对象默认是被关联对象
has_many :comments, -> { where(author_id: 1) }
has_many :employees, -> { joins(:address) }

# scope 里面的查询对象用到自己
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
