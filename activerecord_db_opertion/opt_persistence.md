## Persistence

很重要的模块。

```ruby
# 保存操作
save
save!

# 更新操作
update & update_attributes
update! & update_attributes!
update_attribute
update_column
update_columns

# 删除操作
delete
destroy
destroy!
destroyed?

# 状态询问
new_record?
persisted?

# 更新 updated_at 或指定字段
touch

# 重新加载，清除缓存
reload

# 减一、加一
decrement
decrement!
increment
increment!

# 反转某 bool 属性的值
toggle
toggle!

# 单表继承时，子类对象行为表现像父类对象
becomes
becomes!
```

> Note: 这里大部分是对单个对象的操作。
