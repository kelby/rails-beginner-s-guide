## 其它

---

ArticlesController.helpers

和

ArticlesController._helpers

的区别是什么？

---

不同于其它几个模块，Base 类在这里并不是对外的接口，而是 ActionView 模块本身。因为视图对类和对象的概念没有那么重要。

渲染是它的老本行。

---

lookup_context

```
 @cache=true,
 @details=
  {:locale=>[:en],
   :formats=>[:html, :text, :js, :css, :xml, :json],
   :variants=>[],
   :handlers=>[:erb, :builder, :raw, :ruby]},
 @details_key=nil,
 @prefixes=[],
 @rendered_format=nil,
 @skip_default_locale=false,
 @view_paths=
  #<ActionView::PathSet:0x007ff7250a0fe0
   @paths= ...
```

---

每次都要指定模板文件，并且说一遍渲染，不麻烦吗？<br>
因为模板文件，都在同一目录下，所以我们不必这么麻烦。引进 ActionView 的子模块 Rendering 后我们设置默认的目录即可，并且能自动帮我们"说一遍渲染"。

---

## 为什么有的 helper 我不推荐？

也有一些是我的经验之谈，仅供参考。

- 基于其它 helper，并且功能上十分类似；
- 使用它，写出来的代码反而很丑陋；
- 和样式关联密切，或者CSS、JS也能做的；
- 不实用、或者说不通用

不要迷信 Rails 提供的 helper，有的用起来难读、难懂，还不如自己写。别忘了，设计软件的重点：好读、易维护、以全局观思考。
