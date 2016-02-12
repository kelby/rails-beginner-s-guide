### 更多关于渲染

#### render 在 Controller 和 View 是如何工作的？

**Controller 里默认渲染的是 template**

走的路是 ActionController::Rendering -> AbstractController::Rendering -> ActionView::Rendering -> ActionView::Renderer#render

**View 里默认渲染的是 partial**

走的路是 ActionView::Helpers::RenderingHelper#render -> ActionView::Renderer

通过上面的路径和特别指出的两个 render 方法里面的逻辑，不难看出为什么可以默认渲染 template 或 partial.

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣。

#### 循环渲染单个对象，还是渲染集合

优先渲染一个集合(参数 `:collection`)，而不是一个局部模板或对象。

```ruby
<% @advertisements.each do |ad| %>
  <%= render partial: "ad", locals: { ad: ad } %>
<% end %>

# 建议更改为
<%= render partial: "ad", collection: @advertisements %>
```

可以少敲几个字符，并且 Rails 对渲染集合做了一些优化，性能会变快一点。

另：渲染集合时，尽量不要省略 `:partial` 参数。

#### 其它

Controller 里可以 render 完整的模板(template)、或局部模板(partial)，但 View 里不可指定渲染完整的模板(template).

页面里渲染 js.erb 有时候会有安全隐患，所以渲染后用 `escape_javascript` 处理。
