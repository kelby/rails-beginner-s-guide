## 4 个关联方法的补充

#### 注意事项

只有 :has_many, :has_one, :belongs_to 才有可能自动计算 inverse，也就是说 has_and_belongs_to_many 不可以。

若上述声明里包含 :conditions, :through, :polymorphic, :foreign_key 则同样不可以自动计算 inverse.

#### 所有目标，总结起来就这几个

```ruby
# 实现关联对象

# 调用时传递的 block
define_extensions model, name, &block

# 关联两者
create_reflection model, name, scope, options, extension

# 和关联有关的读写访问器
define_accessors model, reflection

# 和关联有关的回调(删除、自动保存等)
define_callbacks model, reflection

# 和关联有关的校验
define_validations model, reflection

# 对关联对象(特别是对象集合)的增删查改
```
