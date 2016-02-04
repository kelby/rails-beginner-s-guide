## Collection Cache Key

给 Relation 集合也可以有 cache_key，例如：

```ruby
blogs = Blog.where("id > 2");

blogs.collection_cache_key
   (0.2ms)  SELECT COUNT(*) AS size, MAX("blogs"."updated_at") AS timestamp FROM "blogs" WHERE (id > 2)
=> "blogs/query-9a5d6ea830ba519707a5aaf189b9a1b1-0"
```

`ActiveRecord::Base#cache_key` 封装了它，所以直接用 `cache_key` 即可。
