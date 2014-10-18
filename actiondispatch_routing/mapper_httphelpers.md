### HttpHelpers

对应 HTTP 请求里的：

```ruby
delete(*args, &block)
get(*args, &block)
patch(*args, &block)
post(*args, &block)
put(*args, &block)
```

只是语法糖，都是封装了 `match`

```ruby
# 第一个参数 method 表示：delete, get, patch, post, put
def map_method(method, args, &block)
  options = args.extract_options!
  options[:via] = method
  match(*args, options, &block)
  self
end
```
