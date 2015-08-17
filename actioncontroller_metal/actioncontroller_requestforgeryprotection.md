## Request Forgery Protection

防止 CSRF - 跨站请求伪造。

**类方法：**

```
protect_from_forgery
```

使用举例：

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery

  # 跳过 protect_from_forgery
  skip_before_action :verify_authenticity_token, if: :json_request?

  protected

  def json_request?
    request.format.json?
  end
end
```

`protect_from_forgery`
可选参数 :only、:except 和 :with 

当请求未证实，可选择怎么处理。可选：

```ruby
:exception     # 抛 ActionController::InvalidAuthenticityToken 异常。
:reset_session # 重置 session.
:null_session  # 使用空的 session 代替，但不删除(这是默认选择，可用 :with 指定)
```

**实例方法：**

```
verify_authenticity_token
```

```
handle_unverified_request
```

当请求未证实，会使用 `handle_unverified_request` 来处理。
对应上面的 Exception、Reset Session、Null Session.
