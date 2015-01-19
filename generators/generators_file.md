## Generators 文件下的内容

**类方法：**

```
invoke

fallbacks # 添加 fallback

help # 输出帮助信息

no_color! # 在终端里输出没有颜色

subclasses # generators 的子类
public_namespaces

hidden_namespaces
hide_namespaces & hide_namespace

print_generators

sorted_groups
```

`invoke` 授受 namespace, arguments 和 behavior，它是 generate, destroy 和 update 等命令的入口。

`fallbacks` 使用举例：

```ruby
Rails::Generators.fallbacks[:shoulda] = :test_unit
```

**其它类方法：**

```
levenshtein_distance
```

**其它：**

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
    template_engine: '-e',
    test_framework: '-t'
  },

  test_unit: {
    fixture_replacement: '-r',
  }
}

DEFAULT_OPTIONS = {
  rails: {
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
    test_framework: false,
    template_engine: :erb
  }
}
```

```ruby
RAILS_DEV_PATH = File.expand_path("../../../../../..", File.dirname(__FILE__))
RESERVED_NAMES = %w[application destroy plugin runner test]
```
