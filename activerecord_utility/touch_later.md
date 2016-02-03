## Touch Later

提供 `touch_later` 方法。

原来的 `touch` 是一步到位，更新 updated_at 并保存进数据库。
<br>
使用 `touch_later` 后，操作分为了两步：1）更新 updated_at 2）保存进数据库。通常的，它要和事务(transaction)一起使用。
