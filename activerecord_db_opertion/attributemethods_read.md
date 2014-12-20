## Read

`read_attribute` 根据属性名，获取其值。

和 self[:x] 等价

```ruby
read_attribute(:name)

# 等价于

self[:name]
```

数据从 `attributes` 里获取

不同于直接 self.name 它获取的是真实数据

如果字段用于存储图片信息，并且我们有默认图片，则没有图片时：
self.image 返回的是默认图片信息
而 self[:image] 返回 nil 这才是真实信息
这和
self.attributes 里的 image 信息一致

那为什么不用 attributes[:name] 而用 read_attribute[:name] ?
因为性能，前者要把所有的属性都找出来，然后取 name 属性；
而后者可以直接获取 name 属性。

参考

[Prefer self[:attribute] over read_attribute(:attribute)](https://github.com/bbatsov/rails-style-guide#read-attribute)<br>
[Rails attributes() and read_attribute() method](http://www.shanison.com/2010/07/18/rails-attributes-method/)<br>
[How Rails' Type Casting Works](http://robots.thoughtbot.com/how-rails-works-type-casting)<br>

## Write

属性名，加后缀 '=' 进行赋值。

区别于 attr_writer, 这里指的是和数据库操作有关的写。
