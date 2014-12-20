# 其它

在 Helper 和 View 之外，使用 helper 方法。

1) 直接使用

```
OrdersController.helpers.order_number(@order)
ApplicationController.helpers.helper_method
ActionController::Base.helpers.sanitize(str)
```

2) 先引入，再使用

```
include ActionView::Helpers::DateHelper
include ActionView::Helpers
```
