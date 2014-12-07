## Attribute Assignment

```
assign_attributes & attributes=
```

给 record 对象进行赋值，特点是参数的类型是 Hash 对象(这点和 update_columns、update_attributes 类似，但它不会触发 save 操作)。

使用举例：

```ruby
# 直接赋值
cat = Cat.new(name: "Gorby", status: "yawning")
cat.attributes # =>  { "name" => "Gorby", "status" => "yawning", "created_at" => nil, "updated_at" => nil}

# 使用 Attribute Assignment
cat.assign_attributes(status: "sleeping")
cat.attributes # =>  { "name" => "Gorby", "status" => "sleeping", "created_at" => nil, "updated_at" => nil }
```

**对比**

| 方法 | 解释 |
|--|--|
| assign_attributes       | 就是 attributes= |
| update | 就是 update_attributes |
| update | 就是 assign_attributes + save |
