## 其它

### 其它的分类方式

#### 1) 生成内容 vs 影响内容

生成 HTML 内容的为生成内容。

逻辑判断 + 生成内容或纯粹是逻辑，所有不能直观的看出生成什么 HTML 的元素。

#### 2) 单元素 vs 复合元素

单独的 HTML 元素。

生成的内容是 HTML 元素，再包含 HTML 元素。

因为表单实在是太重要了，所以区分一下是否表单元素。

举例：

`link_to` 是生成内容，且是单元素。

`button_to` 是生成内容，且是复合元素。

`link_to_unless_current` 和 `current_page?` 都属于影响内容。

#### 3) 与 record 对象弱关联 vs 与 record 对象强关联

和后台数据关联不大，仅用于生成 HTML 元素。

以 "_tag" 结尾的 helper 都属于这一类，如 AssetTagHelper、FormTagHelper.

除此之外，还有其它，在此不一一列举。

和后台数据关系密切，需要有对象和/或其关联对象。

最典型的如 form_for.

### 在 Helper 和 View 之外，使用 helper 方法。

1) 直接使用：

```ruby
OrdersController.helpers.order_number(@order)
ApplicationController.helpers.helper_method
ActionController::Base.helpers.sanitize(str)
```

2) 先引入，再使用：

```ruby
include ActionView::Helpers::DateHelper
include ActionView::Helpers
```

### 方法太多，看不过来？

有的 Rails 内置方法，难懂、难用，所以可以不用。以 Action View 举例说明：

1. 我们需要看 API，才知道怎么传递参数，有什么参数；
2. 直接写 HTML，大脑不必转一圈；
3. 前端工程师不懂 Ruby，就看不懂；
4. 维护的时候也遇到上述 3 点困境。

