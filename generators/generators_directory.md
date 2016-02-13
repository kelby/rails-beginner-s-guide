## generators 下的目录、文件

对于 Generator, Rails 是自产(定义)和自销(自销)，还对外提供接口，供我们使用。

#### 全部的文件及目录：

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

#### 重要的类：

Base、Named Base 以及 App Base.

#### 自己调用自己：

其中，除 actions/ 和 testing/ 外的其它 5 个目录为调用。

```
css/
erb/
js/assets/
rails/
test_unit/
```

我们在写自己的脚本时，可以参考。

#### 方法、别名、选项默认值

```
api_only!

fallbacks

help

hidden_namespaces & hide_namespace & hide_namespaces

invoke

levenshtein_distance

no_color!

print_generators

public_namespaces

sorted_groups, subclasses
```

别名：

```ruby
DEFAULT_ALIASES = {
  rails: {
    actions: '-a',
    orm: '-o',
    javascripts: '-j',
    javascript_engine: '-je',
    resource_controller: '-c',
    scaffold_controller: '-c',
    stylesheets: '-y',
    stylesheet_engine: '-se',
    scaffold_stylesheet: '-ss',
    template_engine: '-e',
    test_framework: '-t'
  },

  test_unit: {
    fixture_replacement: '-r',
  }
}
```

选项默认值：

```ruby
DEFAULT_OPTIONS = {
  rails: {
    api: false,
    assets: true,
    force_plural: false,
    helper: true,
    integration_tool: nil,
    javascripts: true,
    javascript_engine: :js,
    orm: false,
    resource_controller: :controller,
    resource_route: true,
    scaffold_controller: :scaffold_controller,
    stylesheets: true,
    stylesheet_engine: :css,
    scaffold_stylesheet: true,
    test_framework: false,
    template_engine: :erb
  }
}
```
