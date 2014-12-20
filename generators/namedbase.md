## Named Base √

继承于 Base.

默认 `rails g generator` 生成的就是继承于它。

```
check_class_collision

template

application_name

attributes_names

class_name
class_path

file_path


human_name

i18n_scope

indent
index_helper
inside_template
inside_template?

module_namespacing

namespace
namespaced?
namespaced_class_path
namespaced_file_path
namespaced_path

plural_file_name
plural_name
plural_table_name
pluralize_table_names?

regular_class_path

route_url

singular_name
singular_table_name

table_name

uncountable?

wrap_with_namespace
```

`check_class_collision` 举例 rails generate controller NAME [action action] [options] 检测这里的 NAME 是否已经被使用，也就是说是否有冲突。(因为后续会创建新的文件，如果冲突的话，之前的文件及内容会被覆盖)

```
attr_reader :file_name
```
