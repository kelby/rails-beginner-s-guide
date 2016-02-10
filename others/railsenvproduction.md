## String Inquirer - Rails.env.production?

原来的代码：

```ruby
Rails.env == 'production'
```

语法糖：

```ruby
Rails.env.production?
```

仅作用于 `Rails.env` 对象，方法是动态生成的，所以 API 里查询不到。

手法：把方法最后的问号去掉，看是否和当前环境一样。
