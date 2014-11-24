### Http Helpers

对应 HTTP 请求里的：

```ruby
delete(*args, &block)
get(*args, &block)
patch(*args, &block)
post(*args, &block)
put(*args, &block)
```

只是语法糖，都是封装了 `match`，使用上：

```ruby
# 注意 via 参数
match 'photos/:id' => 'photos#show', via: :get

# 对应着
get 'photos/:id' => 'photos#show'
```
