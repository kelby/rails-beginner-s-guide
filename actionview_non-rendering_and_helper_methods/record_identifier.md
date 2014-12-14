## Record Identifier

根据所传递的对象，生成能代表其身份的"字符串"。

```
dom_class
dom_id
```

可配合其它 helper 一起使用，使用举例：

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

实现它们时直接使用了"字符串求值的"方式，并且用到了 ActiveModel::Model 里的方法。

> Note: 它既没有对应也没有生成 HTML 标签。
