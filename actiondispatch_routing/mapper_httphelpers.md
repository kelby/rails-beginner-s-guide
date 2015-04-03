### Http Helpers

**match 方法的语法糖。**

对应 HTTP 请求里的：

```ruby
delete
get
patch
post
put
```

并且：

```
delete + get + patch + post + put
             |
             V
           match
```

使用上：

```ruby
# 注意 via 参数
match 'photos/:id' => 'photos#show', via: :get

# 对应着
get 'photos/:id' => 'photos#show'
```
