## Attribute Methods 文件下的内容

实例方法：

```
[]
[]=
```

```
attributes
attribute_names

attribute_for_inspect

attribute_present?
has_attribute?
```

另外对某个属性 attribute，会有以下方法：

```ruby
# 在 Read 模块里定义
attribute

# 在 Write 模块里定义
attribute=
```

```ruby
# 在 Query 模块里定义
attribute?
```

一般情况下，如果要覆盖某个属性的读、写方法的话，覆盖的通常是 Read/Write 下的方法，也就是：

```ruby
# 如果要覆盖的话，通常会修改以下方法
user.name
user.name=
```

```ruby
# 以下方法不会覆盖，仍然得到原来的值
user[:name]
user[:name]=
```

像 Enum 提供的 `enum` 和 CarrierWave 提供的 `mount_uploader` 就是这样做的。
