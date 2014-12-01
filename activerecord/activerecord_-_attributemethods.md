## Before Type Cast - 类型转换

同样的数据，每个数据库存储多少有一点不同。
我们'对象.属性'或'类.查询'得到的数据， 未必就是数据库里存放的数据(至少形式上不一样)。
上述两点，虽然差异不大，但多少还是要经过处理的。

举例：对数字和时间，特别是 boolean 类型的数据, 数据库可能用 true/false, t/f, 1/0, T/F 来表示，而我们获取的只有一种 true/false.

```ruby
task.read_attribute('id')                            # => 1
task.read_attribute_before_type_cast('id')           # => '1'

task.read_attribute('completed_on')                  # => Sun, 21 Oct 2012
task.read_attribute_before_type_cast('completed_on') # => "2012-10-21"

task.read_attribute('completed_on')                  # => Sun, 21 Oct 2012
task.completed_on_before_type_cast('completed_on')   # => "2012-10-21"
```

获取一个或所有这样的属性：

```
read_attribute_before_type_cast

attributes_before_type_cast

x_before_type_cast
```

> Note: 默认结果是经过类型转换的。但有时候我们也不希望类型转换，比如在控制台里输出结果，对于'时间'不转换我反而觉得更易读。

---

数据库里存放的和我们读取到的是不一样的，因为经过了类型转换。

```ruby
class Task < ActiveRecord::Base
end

task = Task.new(title: nil, is_done: true, completed_on: '2012-10-21')
task.attributes
# => {"id"=>nil, "title"=>nil, "is_done"=>true, "completed_on"=>Sun, 21 Oct 2012, "created_at"=>nil, "updated_at"=>nil}

task.attributes_before_type_cast
# => {"id"=>nil, "title"=>nil, "is_done"=>true, "completed_on"=>"2012-10-21", "created_at"=>nil, "updated_at"=>nil}
```

再来看看个例子：

```ruby
class Task < ActiveRecord::Base
end

task = Task.new(id: '1', completed_on: '2012-10-21')
task.id           # => 1
task.completed_on # => Sun, 21 Oct 2012

task.attributes_before_type_cast                     # => {"id"=>"1", "completed_on"=>"2012-10-21", ... }
task.read_attribute_before_type_cast('id')           # => "1"
task.read_attribute_before_type_cast('completed_on') # => "2012-10-21"
```

```ruby
# Datetime 默认是不能做校验的，但我们不希望自动转换成 nil，可以先获取它的值。
blog = Blog.new(created_at: 'hello-world')
=> #<Blog id: nil, title: nil, url: nil, created_at: nil, updated_at: nil>

bn.created_at_before_type_cast
=> "hello-world"
```

后面添加的 `_before_type_cast` 尾巴，使用了 ActiveModel 的 `attribute_method_suffix`
