## Form Builder

`default_form_builder`

可以更改表单构造器。（表单本身也是一个对象，通过 Form Builder 添加更多方法）

```ruby
class AdminFormBuilder < ActionView::Helpers::FormBuilder
  def special_field(name)
    # ...
  end
end
```

```ruby
class AdminController < ApplicationController
  default_form_builder AdminFormBuilder
end
```

调用：

```ruby
<%= form_for(@instance) do |builder| %>
  <%= builder.special_field(:name) %>
<% end %>
```
