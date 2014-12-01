# Attribute Methods

## Dirty

跟踪对象值的变化情况。其实，它们都属于脏数据，所以起这名字。

ActiveModel 也有同名 Dirty 模块，这里是对它的使用，并且它并没有对外提供 API.

文档可以参考 ActiveModel::Dirty

## Primary Key

`primary_key` 主键(又称主关键字)。

是表中的一个或多个字段，它的值用于惟一地标识表中的某一条记录。默认是 'id' 属性，一般不会更改。

## Serialization

`serialize` 指定某个字段的存储类型。

这个类型是可序列化的，如：Array，JSON、Hash (此时请注意于 store 方法的区别)

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

## ~~Time Zone Conversion~~
