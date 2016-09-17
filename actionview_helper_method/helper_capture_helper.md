### Capture Helper

存储代码块，`yield` 调用。

```
content_for
content_for?

capture

provide
```

其中 `capture` 和 `content_for` 作用类似，只是定义和调用稍有不同。

使用举例：

```sh
# 定义
<% content_for :navigation do %>
  <li><%= link_to 'Home', action: 'index' %></li>
<% end %>

# 调用
<ul><%= yield :navigation %></ul>

# 或
<ul><%= content_for :navigation %></ul>
```
