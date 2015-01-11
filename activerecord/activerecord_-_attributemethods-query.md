## Query - 后缀 '?' 问询

加后缀 '?' 进行 boolean 判断。

你还在用：

```ruby
<% if @user.login.blank? %>
  <%= link_to 'login', new_session_path %>
<% end %>

# 或

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

每一个 record 属性都有此方法，它可以让我们少敲几个字符。但，除非属性本身就是 boolean 类型，其它类型的判断结果有时候和想像中的不一样，请慎用。不要为了少敲几个字符，增加犯错的几率。

原理是：判断其值是否为 false?、blank? 或 zero? 

```ruby
# status 为 integer 类型的字段，当它为 0 时：
self.status.present? => true
self.status?         => false
```
