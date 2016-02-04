## Collection Cache Key

给 Relation 集合也可以有 cache_key，例如：

```ruby
@users = User.where("name like ?", "%Alberto%")
@users.cache_key
# => "/users/query-5942b155a43b139f2471b872ac54251f-3-20150714212107656125000"
```

`ActiveRecord::Base#cache_key` 封装了它，所以直接用 `cache_key` 也可以。
