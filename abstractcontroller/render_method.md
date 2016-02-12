### 更多关于渲染

#### render 在 Controller 和 View 是如何工作的？

**Controller 里默认渲染的是 template**

走的路是 ActionController::Rendering -> AbstractController::Rendering -> ActionView::Rendering -> ActionView::Renderer#render

**View 里默认渲染的是 partial**

走的路是 ActionView::Helpers::RenderingHelper#render -> ActionView::Renderer

通过上面的路径和特别指出的两个 render 方法里面的逻辑，不难看出为什么可以默认渲染 template 或 partial.

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣。

#### render 单个对象，还是集合

渲染一个集合(传递 :collection)，而不是一个个的渲染对象(传递 :object)。

需要注意：

- 一般要指明 :partial 否则某些情况下会报错。
- 集合里的元素在局部模板里和局部模板同名变量表示。
- 可以用 :as 改变这里的同名变量。(渲染单个对象时，也可以用些参数)
- 额外的参数用 :locals 传递。（上面的同名变量你不用 :as 的话，也可以这么传递）

渲染集合，除了少敲几个字符外，性能也会对应变快一点。

另，渲染集合时 `:partial` 参数，尽量不要省略。

#### 其它

Controller 里可以 render 完整的模板(template)、或局部模板(partial)，但 View 里不可指定渲染完整的模板(template).

页面里渲染 js.erb 有时候会有安全隐患，所以渲染后用 `escape_javascript` 处理。
