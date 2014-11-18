## AttributeAssignment

```
assign_attributes & attributes=
```

不推荐直接使用，因为和直接赋值效果一样，但扩展时可以使用。

使用举例：

```ruby
# 直接赋值
cat = Cat.new(name: "Gorby", status: "yawning")
cat.attributes # =>  { "name" => "Gorby", "status" => "yawning", "created_at" => nil, "updated_at" => nil}

# 使用 Attribute Assignment
cat.assign_attributes(status: "sleeping")
cat.attributes # =>  { "name" => "Gorby", "status" => "sleeping", "created_at" => nil, "updated_at" => nil }
```
