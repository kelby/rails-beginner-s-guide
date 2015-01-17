还有两个类: Default Middleware Stack 和 Routes Reloader. 

它们都放在 application/ 目录下，为了完成某项任务而存在，但两者之间没有联系。

### Default Middleware Stack

`build_stack`

构建默认的 middleware stack，在 Finisher 里有调用代码。

可以通过以下命令查看：

```ruby
Rails.application.send(:default_middleware_stack)
```

### Routes Reloader

充分运用了 File Update Checker，当"路由"相关文件有改动时，可以实现自动重新加载。

开发环境下，不用重启，重新加载路由：

```ruby
Rails.application.reload_routes!
```

相关代码：

```ruby
def reload!
  clear!
  load_paths
  finalize!
ensure
  revert
end
```
