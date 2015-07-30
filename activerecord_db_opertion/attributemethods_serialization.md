## Serialization

`serialize` 指定某个字段的存储类型(默认是 Array)。

这个类型是可序列化的，如：Array，JSON、Hash (此时请注意于【Store】的 `store` 方法的区别)

使用举例：

```ruby
class Post < ActiveRecord::Base
  serialize :title, Hash
end

# 新建对象时，title 相当于 Hash 的名字

post = Post.new
post.title
# => {}

post.title.class
# Hash

post.title = { name: 'Your Name.' }
post.title[:name]
# => Your Name.
```

存储时，如果类型不符合，会报错。

> 名字和【ActiveRecord::Serialization】相同，注意它们的区别。
