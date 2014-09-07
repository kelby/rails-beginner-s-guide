# NamedBase

继承于 Base.

默认 `rails g generator` 生成的就是继承于它。

```
check_class_collision

template

application_name, attributes_names
class_name, class_path
file_path
human_name
i18n_scope, indent, index_helper, inside_template, inside_template?
module_namespacing
namespace, namespaced?, namespaced_class_path, namespaced_file_path, namespaced_path
plural_file_name, plural_name, plural_table_name, pluralize_table_names?
regular_class_path, route_url
singular_name, singular_table_name
table_name
uncountable?
wrap_with_namespace
```

```
attr_reader :file_name
```
