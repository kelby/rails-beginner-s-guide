## 类型转换

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
