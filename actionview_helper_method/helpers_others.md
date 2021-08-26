## 其它

**在 model 里，使用 helper 方法？**

1\) 直接使用：

```ruby
OrdersController.helpers.order_number(@order)
ApplicationController.helpers.helper_method
ActionController::Base.helpers.sanitize(str)
app.helper_method

# Engine
ModuleName::To::EngineName.routes.url_helpers.player_stats_path
```

2\) 先引入，再使用：

```ruby
include ActionView::Helpers::DateHelper
include ActionView::Helpers
```

**方法太多，看不过来？**

有的 Rails 内置方法，难懂、难用，所以可以不用。以 Action View 举例说明：

1. 我们需要看 API，才知道怎么传递参数，有什么参数；
2. 直接写 HTML，大脑不必转一圈；
3. 前端工程师不懂 Ruby，就看不懂；
4. 维护的时候也遇到上述 3 点困境。



