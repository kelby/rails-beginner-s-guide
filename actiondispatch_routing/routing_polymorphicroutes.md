## Polymorphic Routes

### 实例方法

```
polymorphic_url
polymorphic_path
```

1. 不仅仅是多态关联里的'多态'
2. 可根据参数(model 对象)，自动计算生成 url
3. 有几个常用方法是基于它实现的
4. 不用指定具体的路由 helper 方法
5. 对象类型不确定或嵌套对象
6. 同样依赖于路由系统
7. 不要滥用！
8. 复杂度提高，难以理解。

之前：

```ruby
# 在这里 parent 可以是 post 或 news
if Post === parent
  post_comments_path(parent)
elsif News === parent
  news_comments_path(parent)
end
```

之后：

```ruby
# 使用 post_url(post)
polymorphic_url(post)                 # => "http://example.com/posts/1"
polymorphic_url([blog, post])         # => "http://example.com/blogs/1/posts/1"
polymorphic_url([:admin, blog, post]) # => "http://example.com/admin/blogs/1/posts/1"
polymorphic_url([user, :blog, post])  # => "http://example.com/users/1/blog/posts/1"
polymorphic_url(Comment)              # => "http://example.com/comments"
```

当然，多态关联也可用：

```ruby
class Post < ActiveRecord::Base
  has_many :comments
end
class News < ActiveRecord::Base
  has_many :comments
end
class Comment < ActiveRecord::Base
  belongs_to :commentable, :polymorphic => true
end
```

```ruby
polymorphic_path([parent, Comment])
# "/posts/1/comments" 或 "'news/1/comments"

polymorphic_url(parent)
# "http://example.com/posts/1/comments" 或 "http://example.com/news/1/comments"

其它
new_polymorphic_path(Post)  # "/posts/new"
new_polymorphic_url(Post)   # "http://example.com/posts/new"
edit_polymorphic_path(post) # "/posts/1/edit"
edit_polymorphic_url(post)  # "http://example.com/posts/1/edit"
```

**:action 以及其它参数**

举例：

```ruby
# 使用 :aciton 可选参数
polymorphic_path([@user, Document], :action => 'filter')
# => "/users/:user_id/documents/filter"

# 使用 :aciton 可选参数和其它参数
polymorphic_path([@user, Document], :action => 'filter', :sort_order => 'this-order')
# => "/users/:user_id/documents/filter?sort_order=this-order"
```

### 与 url_for 的区别

url_for 比较死板，从它接受的参数就知道了。它不能接受 model 对象 + 可选参数的形式。

url_for 不能直接指定 host, 需要在另一个地方指定，它只有调用的份。如果你为了一个 url_for 而更改这个 host 其它方法或其它 url_for 会不会受影响？

### 其它方法

除上述外，还有方法(元编程生成，API 里查看不到)：

```
# 封装 polymorphic_url 而来
new_polymorphic_url
edit_polymorphic_url

# 封装 polymorphic_path 而来
new_polymorphic_path
edit_polymorphic_path
```

它们封装 polymorphic_url 或 polymorphic_path 而来，所以特点和使用类似。
它们是元编程定义的，所以 API 里看不到。

### 其它

原来这个模块是在 Action Controller 下面的，后面才移到 ActionDispatch::Routing.

我们是可以直接使用这几个方法的。
