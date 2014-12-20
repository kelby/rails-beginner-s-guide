## View Paths

**把路径信息放到 PathSet，之后的 lookup_context 需要它们。**

**实例方法**

```ruby
append_view_path
prepend_view_path

details_for_lookup

lookup_context
```

**类方法**

```
append_view_path
prepend_view_path

view_paths
view_paths=
```

**其它**

```
delegate :template_exists?, :view_paths, :formats, :formats=,
         :locale, :locale=, :to => :lookup_context
```

---

- lookup_context

Lookup Context 查找上下文，很重要概念。它携带着很多信息，如：view paths 和 details，查找模板需要这些东西。
当然，对我们平常使用没有影响，知道有这么回事即可。

- _view_paths

及相关操作，如：prepend_view_path、append_view_path
这个在某些场合还是用得到的。

`append_view_path(path)`<br>
追加路径到当前 Controller 所在的 view paths 里。当我们的自己创建或者使用的 gem 引入的文件目录结构和Rails默认不一样，但且希望渲染到视图里的模板(局部模板)，就会报错，此方法可以帮助我们。

`prepend_view_path` 和 append_view_path 方法类似

---

原来内容是放在 abstract_controller/rendering.rb 文件里的，后来单独成 view_paths.rb，再后来从 Abstract Controller 移到 Action View.
