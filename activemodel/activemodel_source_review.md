## 源码导读

加载 ActionModel 相关 I18n

```ruby
ActiveSupport.on_load(:i18n) do
  I18n.load_path << File.dirname(__FILE__) + '/active_model/locale/en.yml'
end
```
