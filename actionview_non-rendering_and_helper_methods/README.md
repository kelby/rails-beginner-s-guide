# ActionView 非渲染和辅助方法

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
