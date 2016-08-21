## Attribute Assignment

给已有对象的属性进行赋值。

```
assign_attributes
```

是 update & update_attributes 的底层实现。参数的类型都是 Hash 对象，但它不会触发 `save` 操作。

使用举例：

```ruby
# 直接赋值
cat = Cat.new(name: "Gorby", status: "yawning")
cat.attributes # => { "name" => "Gorby", "status" => "yawning", "created_at" => nil, 
                      "updated_at" => nil}

# 使用 Attribute Assignment
cat.assign_attributes(status: "sleeping")
cat.attributes # => { "name" => "Gorby", "status" => "sleeping", "created_at" => nil,
                      "updated_at" => nil }
```

原理上它和直接赋值是一样的（用了元编程一个个属性直接赋值），只是对于要传递的参数多了 ForbiddenAttributesProtection 检查。

对比直接赋值：

```ruby
user.is_admin = true
# 直接赋值，不涉及 ForbiddenAttributesProtection

user.assign_attributes is_admin: true
# ActiveModel::MassAssignmentSecurity::Error:
# Can't mass-assign protected attributes: is_admin
```

我们几乎不会直接使用 `assign_attributes` 给对象赋值。
