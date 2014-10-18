## rails console 里的小技巧

1 `app`  
会话实例，可用于集成测试。

通过它可以做一些你在集成测试里要做的事。

```ruby
app.class
=> ActionDispatch::Integration::Session

# 创建过程, 类似这样
new_app = Rails.application
session = ActionDispatch::Integration::Session.new(new_app)

session.class
=> ActionDispatch::Integration::Session
```

- 可以使用的方法1：所有的 path 和 url (这里不包含其它 helper)
- 可以使用的方法2：request.methods
- 可以使用的方法3：response.methods
- 可以使用的方法4：相关的 asset_.methods
- 可以使用的方法5：ActionController::Base 引入的 private 等方法

2 `new_session`  
返回一个新的集成测试会话实例

3 `reload!`  
重新加载环境。这个应该很熟悉
这个最常用，当修改了 env 配置的时候，需要重新加载。使用这个方法可以不必退出后在启动。

4 `helper`  
ActionView::Base 实例。
通过它可以直接使用 view 的方法等，例如：所有 helper 方法(这里不包含 path 和 url)。

```ruby
module UsersHelper
  def user_help_1(user_name)
    puts "I am June!"
  end

  def user_help_2
    puts "I am June-Lee!"
  end
end

# 原理类似
helper.send :extend, UsersHelper
```

5 `controller`  
ApplicationController 实例。
可以直接使用 ApplicationController 的方法等。
