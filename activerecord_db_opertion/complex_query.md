### 关联表复杂查询示例

#### 单群、复数？

**关键：**

includes、joins 查询对应的是关联名字，where 对应的是表名。

**一对多关系时：**

joins + where 查询会对每个关联进行查询，所以结果会有重复。
<br>
使用 uniq 后前者数目和使用 includes 一样。

includes + where 查询到符合条件就自动终止，所以结果没有重复。
<br>
相当于自带 uniq 功能。

**一对一关系（has_one 或 belongs_to）**

没有上述差异，因为它在另一个层面解决了重复问题。

#### 判断 nil

```ruby
class Person
  has_many :friends
end

class Friend
  belongs_to :person
end
```

对应：

```ruby
Person.includes(:friends).where( :friends => { :person_id => nil } )
```

```ruby
class Person
   has_many :contacts
   has_many :friends, :through => :contacts, :uniq => true
end

class Friend
   has_many :contacts
   has_many :people, :through => :contacts, :uniq => true
end

class Contact
   belongs_to :friend
   belongs_to :person
end
```

对应：

```ruby
Person.includes(:contacts).where( :contacts => { :person_id => nil } )
```

另外一种解法：

```ruby
Person.includes(:contacts).where( :contacts => { :id => nil } )
```

还有一种丑陋、低效的办法：

```ruby
# 取出所有（去重），不在里面则表示 nil
Person.where('id NOT IN (SELECT DISTINCT(person_id) FROM friends)')
```

如果把 has_many 改为 has_one 呢？

```ruby
Person.includes(:contact).where( :contacts => { :person_id => nil } )
```

#### 判断非 nil

以下几个查询几乎等价：

```ruby
members.includes(:responses).where('responses.id IS NOT NULL')

members.includes(:responses).where.not(responses: { id: nil })

members.joins(:responses)
```

看情况决定是否要加 uniq 条件。

#### 关联的关联

```ruby
class Employee < ActiveRecord::Base
  belongs_to :company
end

class Company < ActiveRecord::Base
  has_many :employees
  has_many :addresses
end

class Address < ActiveRecord::Base
  belongs_top :company
end
```

```ruby
Employee.joins(:company => :addresses).
  where(:addresses => { :city => 'Porto Alegre' })
```

#### 关联对象的数量

**一对一**

直接上是不会发生的，但实际上，如果没有删除关系对象的话，也会有：

group + having

```ruby
Order.joins( :request_refund ).group( 'orders.id' ).having( 'count( order_id ) > 1' )
```

**一对多**

group + having

```ruby
Order.joins( :request_refunds ).group( 'orders.id' ).having( 'count( order_id ) > 1' )
```

counter_cache

```ruby
belongs_to :project, counter_cache: true
```

当使用了 `counter_cache` 时可以先运行 `reset_counters` 确保数据准确，然后运用计数器：

```ruby
Project.where( 'vacancies_count > ?', 1 )
```

#### 统计并排序

```ruby
users = User.where('tag_list != ?', '').group(:tag_list).order('count_all desc').count
```

#### 按指定的数组顺序排列

```ruby
Model.where(id: ids).order("field(id, #{ids.joins(',')})")
```