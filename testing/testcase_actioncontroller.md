## Action Controller

#### Test Case Behavior

实例方法：

```
get
post
patch
put
delete
head

xhr & xml_http_request

process
```

> Rails 5 新增 xhr 虽然是语法糖，但用起来很舒心，不是吗？

#### Template Assertions

> Rails 5 已废除。

```
assert_template
```

此外，还包含了：Test Request、Test Response、Live Test Response、Test Session.
<br>
这几个类作用上比较小，再此不做讲解。

> ActionController::TestCase 在 Rails 5.1 将会被完全移除，使用 ActionDispatch::IntegrationTest 代替。
