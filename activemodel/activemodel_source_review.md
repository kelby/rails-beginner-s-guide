## 源码导读

加载 ActionModel 相关 I18n

```ruby
ActiveSupport.on_load(:i18n) do
  I18n.load_path << File.dirname(__FILE__) + '/active_model/locale/en.yml'
end
```
**Validator & EachValidator**

还包括 BlockValidator

**Validations & validations 目录**

校验相关方法。

**Translation**

**Serialization**

**Railtie & TestCase**

基本上都是空的。
