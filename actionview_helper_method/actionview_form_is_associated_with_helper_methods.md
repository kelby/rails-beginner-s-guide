# ActionView 与对象是否相关的辅助方法

如何處理 Model 中不存在的屬性

使用 form_for 時，其中的欄位必須是 Model 有的屬性，那如果資料庫沒有這個欄位呢?這時候你依需要在 Model 程式中加上存取方法，例如：

```ruby
class Event < ActiveRecord::Base
  #...
  def custom_field
      # 根據其他屬性的值或條件，來決定這個欄位的值
  end

  def custom_field=(value)
      # 根據value，來調整其他屬性的值
  end
end
```

這樣就可以在form_for裡使用custom_field了。

```ruby
<%= form_for @event do |f| %>
  <%= f.text_field :custom_field %>
  <%= f.submit %>
<% end %>
```

