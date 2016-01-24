### 跳过回调

主要有 4 种方式：

**update_columns 和 update_column**
<br>
还有 update_column，这些都是直接执行 SQL 语句，不会触发回调方法。

使用举例：

```ruby
user.update_columns(last_request_at: Time.current)
```

**save(:validate => false)**

跳过 Model 里的所有校验。

**skip_callback**
<br>
跳过某个回调。

使用举例：

```ruby
class Writer < Person
  skip_callback :validate, :before, :check_membership, if: -> { self.age > 18 }
end
```

**x_without_callbacks**
<br>
以 object.send(:x_without_callbacks)
跳过某个系列的回调。

使用举例：

```ruby
object.send(:create_without_callbacks)
object.send(:update_without_callbacks)
```
