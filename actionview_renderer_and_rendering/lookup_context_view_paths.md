### View Paths

实例方法：

```
find & find_template
exists? & template_exists?

find_all

view_paths=

with_fallbacks
```

`find` 很重要的方法，用来查找模板(Template)。

`template_exists?` 使用举例，在视图里检查局部模板是否存在：

```
template_exists?("form", "posts", true)
```

> Note: 区别于【ActionView::ViewPaths】对应的模块。
