## Routes Reloader

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

这部分，更多信息可以查看"Active Support autoload 的类和模块"下面的【File Update Checker】章节。
