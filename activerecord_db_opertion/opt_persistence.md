## Persistence

很重要的模块。

```
# 减一、加一
decrement
decrement!
increment
increment!

# 删除操作
delete
destroy
destroy!
destroyed?

# 状态询问
new_record?
persisted?

# 重新加载，清除缓存
reload

# 保存操作
save
save!

# 反转某 bool 属性的值
toggle
toggle!

# 更新 updated_at 或指定字段
touch

update & update_attributes
update! & update_attributes!

update_attribute

update_column
update_columns

becomes
becomes!
```

> Note: 这里大部分是对单个对象的操作。
