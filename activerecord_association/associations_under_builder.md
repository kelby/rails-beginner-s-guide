## Builder - 功能的实现

实际指的是 Builder Associations，关联方法带来的基本方法。

#### 主要做 6 件事

```ruby
# 调用时传递的 block
define_extensions model, name, &block

# 关联两者
create_reflection model, name, scope, options, extension

# 读写访问器
define_accessors model, reflection

# 和关联有关的回调
define_callbacks model, reflection

# 和关联有关的校验
define_validations model, reflection

# 对关联对象的增删查改
```

#### 关系图

```
  HasOne   &   BelongsTo        HasMany   &   HasAndBelongsToMany
           |                              |
           V                              V
    SingularAssociation      &     CollectionAssociation
                             |
                             V   
                         Association
```

#### 其它

- 参数相关处理：
  - 参数是否合法  
  - 中间表(join_table, class_name, source 及多对多关系的自动生成...等)  
  - autosave 相关实现

> Note: 对关联表的处理时，大量使用了 reflection 里面的实例方法。
