## Polymorphic Routes

**多态路由辅助方法。**

#### 实例方法

```
polymorphic_url
polymorphic_path
```

1. 不仅仅是多态关联里的'多态'
2. 可根据参数(record 对象)，自动计算生成 url
3. 有几个常用方法是基于它实现的
4. 不用指定具体的路由 helper 方法
5. 对象类型不确定或嵌套对象
6. 同样依赖于路由系统
7. 使用它会使得复杂度提高，难以理解，所以不要滥用

使用之前的做法：

```ruby
# 在这里 parent 可以是 post 或 news
if Post === parent
  post_comments_path(parent)
elsif News === parent
  news_comments_path(parent)
end
```

使用之后：

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
# "/posts/1/comments"
# 或
# "news/1/comments"

polymorphic_url(parent)
# "http://example.com/posts/1/comments"
# 或
# "http://example.com/news/1/comments"

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

#### 与 url_for 的区别

url_for 比较死板，从它接受的参数就知道了。它不能接受 record 对象 + 可选参数的形式。

url_for 不能直接指定 host, 需要在另一个地方指定，它只有调用的份。如果你为了一个 url_for 而更改这个 host 其它方法或其它 url_for 会不会受影响？

#### 其它方法

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

#### ~~Helper Method Builder~~

以字符串拼接为手段，得到具体的路由 helper 方法(不能直接得到网址！)。后续，可以再通过路由 helper 方法，得到网址。

:action - 其实是生成的方法所带的前缀，默认是没有的。Rails 使用了前缀 :new 和 :edit

:routing_type - 生成 :path 还是 :url, 默认是 :url.

```ruby
ActionDispatch::Routing::PolymorphicRoutes::HelperMethodBuilder.plural 'edit', 'url'

when Array
builder.handle_list record   # 拆分处理，方式雷同
  when String, Symbol
builder.handle_string record # 直接使用此字符串
  when Class
builder.handle_class record  # 小写，复数形式
  else
builder.handle_model record  # 类型，小写，单数形式
```

现在可以看到 Action View 和 Action Dispatch 里的 url_for 方法, 以及 Action Dispatch 里的 Polymorphic Routes 相关方法都封装了它，在某些参数情况下会调用到它。其它地方，没有使用。

#### 其它

原来这个模块是在 Action Controller 下面的，后面才移到 ActionDispatch::Routing.

我们是可以直接使用这几个方法的。
