## Touch Later

提供 `touch_later` 方法。

它和事务（transaction）一起使用，把原来的 `touch` 操作分为了两步。1）更新 updated_at 2）保存进数据库