## Spawn Methods

| 方法 | 解释 |
| --- | --- |
| except | 查询方法有多个，并且可以链式调用。使用 except 忽略之前的某个查询方法 |
| merge | 参数是 Relation, 则做为查询条件，返回仍然是 Relation；参数是数组, 则返回前者的查询结果和此数组的交集 |
| only | 查询方法有多个，并且可以链式调用。使用 only 指定只能使用的查询方法 |

当参数是 Relation 时，merge 也和 joins、includes 等一样有联合查询的效果。

`except` 和 `unscope` 功能上类似。区别在于后者在使用上，可以选择更多类型。

除上述方法外，还有：

```
spawn

merge!
```

```ruby
VALID_FIND_OPTIONS = [ :conditions, :include, :joins, :limit, :offset, :extend,
                       :order, :select, :readonly, :group, :having, :from, :lock ]
```

**一种 merge 转普通查询：**

之前

```ruby
User.joins(:account).merge(Account.where(:active => true))
```

之后

```ruby
User.joins(:account).where(:accounts => { :active => true })
```

并不是所有 merge 都可转换成 where，但能转换的话，请务必优先用 where.
