# Application 其它

## Default Middleware Stack

`build_stack`

构建默认的 middleware stack，在 Finisher 里有调用代码。

可以通过以下命令查看：

```ruby
Rails.application.send(:default_middleware_stack)
```

## Routes Reloader*

`Rails.application.reload_routes!`

仅是开发环境下，仅对这条命令的处理。(除此之外，无用)

```ruby
def reload!
  clear!
  load_paths
  finalize!
ensure
  revert
end
```
