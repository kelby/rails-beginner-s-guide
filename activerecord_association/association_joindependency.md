### ~~Join Dependency~~

**作用**：为 Finder Methods 和 Query Methods 服务。

实现 joins

```ruby
class Physician < ActiveRecord::Base
   has_many :appointments
   has_many :patients, through: :appointments
end
```

以下两种查询对应的 joins 语句是不同的：

```
@physician.patients.to_a

Physician.joins(:appointments).to_a
```
