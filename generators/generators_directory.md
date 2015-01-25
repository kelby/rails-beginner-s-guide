## generators 下的目录、文件

对于 Generator, Rails 是自产(定义)和自销(自销)，还对外提供接口，供我们使用。

### 全部的自产和自销：

```
# 目录
css/
erb/
js/assets/
rails/
test_unit/

# 文件
actions.rb
active_model.rb
app_base.rb
base.rb
erb.rb
generated_attribute.rb
migration.rb
model_helpers.rb
named_base.rb
resouce_helpers.rb
test_case.rb
test_unit.rb

# 目录
actions/
testing/
```

### 重要的自产：

Base、Named Base 以及 App Base.

### 所有自销：

其中，除 actions/ 和 testing/ 外的其它 5 个目录为调用。

```
css/
erb/
js/assets/
rails/
test_unit/
```
