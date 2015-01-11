## Core

从原来的 Base 中抽取出来单独成 module，负责部分底层对接、重写部分常用方法等。一起抽取出来的还有 Model 模块(包含 include、extend 等，现已经被删除)

**包含时即执行：**

```
mattr_accessor :logger, instance_writer: false

self.configurations

mattr_accessor :default_timezone, instance_writer: false
mattr_accessor :schema_format, instance_writer: false
mattr_accessor :timestamped_migrations, instance_writer: false
mattr_accessor :dump_schema_after_migration, instance_writer: false
mattr_accessor :maintain_test_schema, instance_accessor: false

class_attribute :default_connection_handler, instance_writer: false
class_attribute :find_by_statement_cache
```

**实例方法：**

```
<=>
== & eql?

clone
dup

connection_handler

encode_with
init_with

freeze
frozen?

hash

inspect

pretty_print

readonly!

readonly?

slice
```

`self.allocate` 和 `init_with` 使用举例：

```ruby
class Post < ActiveRecord::Base
end

post = Post.allocate
post.init_with('attributes' => { 'title' => 'hello world' })
post.title # => 'hello world'
```

**类方法：**

```
===

allocate

find
find_by
find_by!

generated_association_methods

inherited

initialize_find_by_cache
initialize_generated_modules

inspect
```
