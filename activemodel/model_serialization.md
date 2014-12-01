## Serialization

`serializable_hash(options = nil)` 序列化操作，提供了 :only, :except 选项。常用的 `as_json` 封装并扩展了它。

使用 Serialization 需要 record.respond_to? :attributes # => true 因为转换的过程使用到相应 model 的 attributes 实例方法。

此外，还有相关 module 及方法：

### JSON

```ruby
# 封装了上面的 serializable_hash，提供 :root 选项 
as_json

# 调用了 attributes= 方法，给对象赋值
from_json
```

### Xml

```ruby
# 将对象转化成 xml 格式
to_xml

# 先将 xml 格式成 Hash，再调用了 attributes= 方法，给对象赋值
from_xml
```

**注意几个转化成 json 方法之间的区别**

```ruby
person = Person.new
person.name = "Bob"
person.serializable_hash # => {"name"=>"Bob"}
person.as_json           # => {"name"=>"Bob"}
person.to_json           # => "{\"name\":\"Bob\"}"
```  
