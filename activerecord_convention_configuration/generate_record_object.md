### 获取 record 对象

归结起来，有两种方式可以获得 record 对象：**1) 从数据库里获取 2) 程序新创建**。

前者可以通过各种方法查询得到，但都是封装了 `init_with`
<br>
后者也可以通过各种方法创建得到，但都是封装了 `initialize`

```ruby
# 查询得到。从 SQL 层面转向到 Ruby 层面
def find_by_sql(sql, binds = [])
  result_set = connection.select_all(sanitize_sql(sql), "#{name} Load", binds)
  column_types = result_set.column_types.dup

  # ...

  result_set.map { |record| instantiate(record, column_types) }
end
```

`instantiate` 查询得到，等价于：`self.allocate` + `init_with`

```ruby
def instantiate(attributes, column_types = {})
  klass = discriminate_class_for_record(attributes)
  attributes = klass.attributes_builder.build_from_database(attributes, column_types)
  klass.allocate.init_with('attributes' => attributes, 'new_record' => false)
end

def allocate
  define_attribute_methods
  super
end

def init_with(coder)
  @attributes = coder['attributes']

  init_internals

  @new_record = coder['new_record']

  self.class.define_attribute_methods

  _run_find_callbacks
  _run_initialize_callbacks

  self
end
```

`initialize` 创建得到，比如：`new` 或 `build`

```ruby
User.new(first_name: 'Jamie')
```

```ruby
def initialize(attributes = nil, options = {})
  @attributes = self.class._default_attributes.dup
  self.class.define_attribute_methods

  init_internals
  initialize_internals_callback

  init_attributes(attributes, options) if attributes

  yield self if block_given?
  _run_initialize_callbacks
end
```

前者，也就是查询得到，一定有 `persisted?` # => true
<br>
后者，也就是创建得到，一定有 `persisted?` # => false

其它类似：

```
clone
dup

initialize_dup
```

`clone`

```
user = User.first
new_user = user.clone
user.name               # => "Bob"
new_user.name = "Joe"
user.name               # => "Joe"

user.object_id == new_user.object_id            # => false
user.name.object_id == new_user.name.object_id  # => true

user.name.object_id == user.dup.name.object_id  # => false
```

`encode_with`

```
class Post < ActiveRecord::Base
end
coder = {}
Post.new.encode_with(coder)
coder # => {"attributes" => {"id" => nil, ... }}
```

`init_with`

```
class Post < ActiveRecord::Base
end

post = Post.allocate
post.init_with('attributes' => { 'title' => 'hello world' })
post.title # => 'hello world'
```

参考

[ActiveRecord 对象的拼装](http://thekaiway.com/2013/07/26/assemble-ar-object/)
<br>
[Read and Write Activerecord Attribute](http://thekaiway.com/2013/09/08/read-write-activerecord-attribute/)
<br>
[ActiveRecord Object Instantiate](https://ruby-china.org/topics/23523)
<br>
[Using Allocate to Create ActiveRecord Objects When Stubbing find_by_sql](http://neovintage.blogspot.jp/2010/10/using-allocate-to-create-activerecord.html)
