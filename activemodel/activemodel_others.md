## 回调及其顺序

每个操作，它所对应的回调(按顺序来的)。

### 创建

```
before_validation
after_validation
before_save
around_save
before_create
around_create
after_create
after_save
after_commit/after_rollback
```

### 更新

```
before_validation
after_validation
before_save
around_save
before_update
around_update
after_update
after_save
after_commit/after_rollback
```

### 删除

```
before_destroy
around_destroy
after_destroy
after_commit/after_rollback
```

**save = create + update**

**commit = create + update + destroy** 当然也包含了 save 在内

> Note: 执行 create 和 update 操作，都会触发 after_save 回调。但它的顺序始终在 after_create 和 after_update 之后。即使在 model 里它定义在前面，效果一样。

### after_initialize 和 after_find

不管是直接 new 或其它途径，只要有对象被初始化，就会触发 after_initialize 回调。使用它，可以避免重写 initialize 方法。

只要从数据库里查找记录，就会触发 after_find 回调。并且，after_find 和 after_initialize 同时定义的时候，after_find 优先级要高于 after_initialize.

initalize 和 find 只有 after_* 回调，也就是 after_initialize 和 after_find callbacks，没有对应的 before_* 回调。但它的用法和其它的回调一 样：

```ruby
class User < ActiveRecord::Base
  after_initialize do |user|
    puts "You have initialized an object!"
  end

  after_find do |user|
    puts "You have found an object!"
  end
end

>> User.new
You have initialized an object!
=> #<User id: nil>

>> User.first
You have found an object!
You have initialized an object!
=> #<User id: 1>
```

> Note: 上面例子已经证明，从数据库里查找记录，也会有新的对象创建，所以会有 initialize 过程。

### after_touch

对对象执行 touch 更新后，都会运行 after_touch 回调方法。我们可以指定其内容：

```ruby
class User < ActiveRecord::Base
  after_touch do |user|
    puts "You have touched an object"
  end
end

>> u = User.create(name: 'Kuldeep')
=> #<User id: 1, name: "Kuldeep", created_at: "2013-11-25 12:17:49", updated_at: "2013-11-25 12:17:49">

>> u.touch
You have touched an object
=> true
```

在 belongs_to 关联里，除 `touch: true` 外，使用 after_touch 可以做更多的操作。

```ruby
class Employee < ActiveRecord::Base
  belongs_to :company, touch: true
  after_touch do
    puts 'An Employee was touched'
  end
end

class Company < ActiveRecord::Base
  has_many :employees
  after_touch :log_when_employees_or_company_touched

  private
  def log_when_employees_or_company_touched
    puts 'Employee/Company was touched'
  end
end

>> @employee = Employee.last
=> #<Employee id: 1, company_id: 1, created_at: "2013-11-25 17:04:22", updated_at: "2013-11-25 17:05:05">

# triggers @employee.company.touch
>> @employee.touch
Employee/Company was touched
An Employee was touched
=> true
```

> Note: after_touch 实际上运用得比较少。执行 touch 操作，除它之外，还会触发 after_commit 和 after_rollback 回调函数。

### 查看某个记录关联的回调及其顺序

以 save 举例：

```
after_save :回调 1, :回调 2
```

查看某个记录关联的回调及其顺序

```
a_record._save_callbacks.map{ |c| puts c.raw_filter };

=>
  save 回调 1
  save 回调 2
  ... ...
  save 回调 3
  save 回调 4
```

执行顺序从下往上，所以回调之间有相同内容的话，上面的可以覆盖下面的。但如果下面的回调执行失败的话，也会影响到上面的。

这里不区分是系统生成的方法(大多数是关联时就带有，如 autosave_associated_records、belongs_to_counter_cache、has_many_dependent)，还是我们自定义的方法。

用 `:prepend` 参数可以更改回调在堆栈里的顺序，如：

```ruby
class Foo < ActiveRecord::Base
  belongs_to :bar, autosave: true

  before_save :modify_bar, prepend: true
end
```

在这里，autosave 在 modify_bar 之前完成，也就是说 modify_bar 优先级高。
