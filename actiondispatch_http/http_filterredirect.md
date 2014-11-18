## ~~Filter Redirect~~

```
filtered_location
```

配置 config.filter_redirect，可以从日志里过滤掉一些敏感的"重定向地址"：

```ruby
config.filter_redirect << ''www.rubyonrails.org'
```

可以使用字符串、正则表达式，或者一个数组(包含字符串或正则表达式)：

```ruby
config.filter_redirect.concat [''www.rubyonrails.org', /private_path/]
```

匹配的 URL 会显示为 '[FILTERED]'。

查看应用程序过滤了什么字段：

```ruby
Rails.application.config.filter_redirect
```
