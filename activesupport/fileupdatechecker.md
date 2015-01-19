## File Update Checker

**文件更新检测**(文件更新后，不用重启自动编译、生效)。

Rails 默认有 3 个：

- i18n_reloader(I18n 相关)
- routes updater(Routes 相关)
- file_watcher(其它所有)

我们定制的可以直接使用 Rails.configuration.file_watcher 放到"其它所有"里，也可以直接调用 ActiveSupport::FileUpdateChecker.

```ruby
# 定义
i18n_reloader = ActiveSupport::FileUpdateChecker.new(paths) do
  I18n.reload! # 要执行的内容
end

# 调用
ActionDispatch::Reloader.to_prepare do
  i18n_reloader.execute_if_updated
end

# 调用
i18n_reloader.execute
```

用 Rails.application.reloaders 查看所有检测器(要先放进来，才能查看)。

```ruby
Rails.application.reloaders << i18n_reloader
```

除上述方法外，还有：

```
updated?
```
