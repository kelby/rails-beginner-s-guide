# Metal 使用举例

Controller 里的 render 是为了返回 self.response_body
而 View 里的 render 则好像为了渲染而渲染，返回的不再是单纯的 self.response_body。它们只是名字相同而矣


在 Rails 里 metal 也属于 middleware，我们可以这么用：

```ruby
# config/routes.rb
  get 'hello' => 'hello#index'
  ...
```

```ruby
# app/controllers/hello_controller.rb
class HelloController < ActionController::Metal
  def index
    self.response_body = "Hello World!"
  end
end
```

然后浏览器访问：http://localhost:3000/hello 可以获取刚才的内容。

日志
    Started GET "/hello" for 127.0.0.1 at 2014-04-27 08:57:07 +0800

改为

Started GET "/hello" for 127.0.0.1 at 2014-04-27 09:04:49 +0800
Processing by HelloController#index as HTML
Completed 200 OK in 1ms (ActiveRecord: 0.0ms)

