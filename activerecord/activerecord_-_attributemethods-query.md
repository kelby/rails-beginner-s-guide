加后缀 '?' 进行询问。

你还在用：

```ruby
<% if @user.login.blank? %>
  <%= link_to 'login', new_session_path %>
<% end %>

# Object#present? is the same thing as calling !obj.blank?.
<% if @user.login.present? %>
  <%= @user.login %>
<% end %>
```

你 Out 了，直接：

```ruby
<% unless @user.login? %>
  <%= link_to 'login', new_session_path %>
<% end %>

<% if @user.login? %>
  <%= @user.login %>
<% end %>
```

Each attribute of ActiveRecord's model has a query method, so you don't need to use the present? or blank? for ActiveRecord's attributes.

> Note: 对应着 ActiveModel::AttributeMethods::Query，原理是判断其值是否为 blank? 或 zero?

[Use query attribute](http://rails-bestpractices.com/posts/56-use-query-attribute)
