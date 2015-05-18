## File Update Checker

**文件更新检测**(文件更新后，不用重启自动编译、生效)。

Rails 默认有 3 个：

- i18n_reloader(I18n 相关)
- routes updater(Routes 相关)
- file_watcher(其它所有)

我们定制的可以直接使用 Rails.configuration.file_watcher 放到"其它所有"里，也可以直接调用 ActiveSupport::FileUpdateChecker.

```ruby
# 定义。这里的 paths 表示你要监视的文件或目录(所在的路径)。
i18n_reloader = ActiveSupport::FileUpdateChecker.new(paths) do
  I18n.reload! # 监视内容有变化时，执行什么操作。
  # Rails.application.reload_routes!
end

ActionDispatch::Reloader.to_prepare do # to_prepare 的作用，可参考对应说明。
  # 调用。如果有更新，则执行操作
  i18n_reloader.execute_if_updated
end

# 调用。手动执行操作
i18n_reloader.execute
```

可用 Rails.application.reloaders 查看所有检测器(要先放进来，才能查看)。

```ruby
Rails.application.reloaders << i18n_reloader
```

除上述方法外，还有：

```
updated?
```

用于询问监视的文件/目录是否有变化。
