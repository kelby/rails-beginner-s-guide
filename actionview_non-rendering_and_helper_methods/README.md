# ActionView 非渲染 & 非辅助方法

## RecordIdentifier

根据所传递的对象，生成能代表其身份的"字符串"，可配合其它 helper 一起使用。

使用举例：

```ruby
dom_class(post)   # => "post"
dom_class(Person) # => "person"

# 带前缀
dom_class(post, :edit)   # => "edit_post"
dom_class(Person, :edit) # => "edit_person"
```

```ruby
dom_id(Post.find(45))       # => "post_45"
dom_id(Post.new)            # => "new_post"

# 带前缀
dom_id(Post.find(45), :edit) # => "edit_post_45"
dom_id(Post.new, :custom)    # => "custom_post"
```

实现它们时用到了 ActiveModel::Model 里的方法。

## RoutingUrlFor

`url_for`

视图里的 url_for，极端情况下会用到 ActionDispatch 里的东西。

有多个 url_for 方法，这里指的是 View 里用到的。

它生成的内容是 url，区别于 link_to 等生成的是链接。
