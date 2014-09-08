## rails console 里的小技巧

1 `app`  
集成测试会话实例。
通过它可以做一些你在集成测试里要做的事。

```ruby
app.class
=> ActionDispatch::Integration::Session

# 创建过程类似这样
new_app = Rails.application
session = ActionDispatch::Integration::Session.new(new_app)

session.class
=> ActionDispatch::Integration::Session
```

- 可以使用的方法1：模拟请求
- 可以使用的方法2：单元测试
- 可以使用的方法3：命名路由
- 可以使用的方法4：属性方法
- 可以使用的方法5：ActionController::Base 引入的方法

2 `new_session`  
返回一个新的集成测试会话实例

3 `reload!`  
重新加载环境。这个应该很熟悉
这个最常用，当修改了env配置的时候，需要重新加载。使用这个方法可以不必退出后在启动。

4 `helper`  
ActionView::Base 实例。
通过它可以直接使用 view 的方法等，例如：helper 方法等。

```ruby
module UsersHelper
  def user_help_1(user_name)
    puts "I am June!"
  end

  def user_help_2
    puts "I am June-Lee!"
  end

end
helper.send :extend, UsersHelper
```

5 `controller`  
ApplicationController 实例。
可以直接使用 ApplicationController 的方法等。

---

6 `source_location`

```ruby
>> Project.instance_method(:trash).source_location
=> ["/Users/qrush/37s/apps/bcx/app/models/project.rb", 90]

>> app.method(:get).source_location
=> ["/Users/qrush/.rbenv/versions/1.9.3-p194/lib/ruby/gems/1.9.1/bundler/gems/rails-7d95b814583b/actionpack/lib/action_dispatch/testing/integration.rb", 32]
```
