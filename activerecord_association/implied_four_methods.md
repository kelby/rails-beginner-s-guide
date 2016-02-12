## 4 个关联方法的补充

#### 注意事项

只有 :has_many, :has_one, :belongs_to 才有可能自动计算 inverse，也就是说 has_and_belongs_to_many 不可以。

若上述声明里包含 :conditions, :through, :polymorphic, :foreign_key 则同样不可以自动计算 inverse.
