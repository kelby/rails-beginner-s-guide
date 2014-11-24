# Attribute Methods

## Before Type Cast

同样的数据，每个数据库存储多少有一点不同。
我们'对象.属性'或'类.查询'得到的数据， 未必就是数据库里存放的数据(至少形式上不一样)。
上述两点，虽然差异不大，但多少还是要经过处理的。

举例：对数字和时间，特别是 boolean 类型的数据, 数据库可能用 true/false, t/f, 1/0, T/F 来表示，而我们获取的只有一种 true/false.

```
task.read_attribute('id')                            # => 1
task.read_attribute_before_type_cast('id')           # => '1'
task.read_attribute('completed_on')                  # => Sun, 21 Oct 2012
task.read_attribute_before_type_cast('completed_on') # => "2012-10-21"
```

获取一个或所有这样的属性：

```
read_attribute_before_type_cast
attributes_before_type_cast
```

> Note: 对于普通 Web 开发者而言，几乎用不到它们。

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

## Write

属性名，加后缀 '=' 进行赋值。

区别于 attr_writer, 这里指的是和数据库操作有关的写。
