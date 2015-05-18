## View Paths

把路径信息放到 PathSet，之后的 lookup_context 需要它们。

**实例方法：**

```ruby
append_view_path
prepend_view_path

details_for_lookup

lookup_context
```

**类方法：**

```
append_view_path
prepend_view_path

view_paths
view_paths=
```

`append_view_path(path)` 追加路径到当前 Controller 所在的 view paths 里。

当我们的自己创建或者使用的 gem 引入的文件目录结构和 Rails 默认不一样，但且希望渲染到视图里的模板(局部模板)，就会报错，此方法可以帮助我们。

`prepend_view_path` 和 append_view_path 方法类似。

**其它：**

```
delegate :template_exists?, :view_paths, :formats, :formats=, :locale, :locale=,
         :to => :lookup_context
```

这里的内容，原来是放在 abstract_controller/rendering.rb 文件里的，后来单独成 view_paths.rb，再后来从 Abstract Controller 移到 Action View.

> Note: 区别于【ActionView::LookupContext::ViewPaths】
