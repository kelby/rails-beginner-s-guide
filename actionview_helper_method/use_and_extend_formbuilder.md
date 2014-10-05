# 使用和扩展 Form Builder

**使用:**

```ruby
<%= form_for @person do |f| %>
  Name: <%= f.text_field :name %>
  Admin: <%= f.check_box :admin %>
<% end %>
```

这里的 `f` 是 FormBuilder 的实例对象，所以可以直接调用 FormBuilder 提供的方法。

**扩展:**

```ruby
class MyFormBuilder < ActionView::Helpers::FormBuilder
  def div_radio_button(method, tag_value, options = {})
    @template.content_tag(:div,
      @template.radio_button(
        @object_name, method, tag_value, objectify_options(options)
      )
    )
  end
end
```

```ruby
<%= form_for @person, :builder => MyFormBuilder do |f| %>
  I am a child: <%= f.div_radio_button(:admin, "child") %>
  I am an adult: <%= f.div_radio_button(:admin, "adult") %>
<% end -%>
```
