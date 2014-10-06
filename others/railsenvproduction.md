## Rails.env.xxx?

举例：Rails.env.production?

仅作用于 Rails.env 对象<br/>
是 Rails.env == 'production' 的语法糖<br/>
动态生成的，所以 API 里查询不到<br/>
手法是：把方法最后的问号去掉，看是否和当前环境一样

## StringInquirer

```ruby
Rails.env == 'production'
```

语法糖

```ruby
Rails.env.production?
```

实现原理：method_missing 然后检查方法最后是不是以 ? 结尾，再用 == 判断。
评价：真的只是语法糖，并且局限于 Rails.env 对象，所以实际意义不大。
