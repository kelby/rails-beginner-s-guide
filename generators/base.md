## Base

继承于 Thor::Group 并 include Rails::Generators::Actions

#### 类方法

```ruby
base_root
default_source_root
desc
hide!
namespace
remove_hook_for

hook_for

source_root
```

`source_root` generator 示例模板文件(templates)所在位置。

```ruby
# 默认在 templates 目录下
source_root File.expand_path("../templates", __FILE__)
```

`hook_for` 触发其它的 generator，以 --option 的形式提供。

`desc` 终端里使用对应命令获取帮助(即 --help)会在最后显示此内容。

#### 其它类方法

```
add_shebang_option!  
banner
base_name  

default_aliases_for_option
default_for_option
default_generator_root
default_value_for_option

generator_name  
usage_path
```

#### 实例方法

```
extract_last_module
```

#### 其它

```
filename_with_extensions
```
