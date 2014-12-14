**Show Exceptions**

程序抛异常时，用什么程序来处理。

它只是接口，它关心的是用什么，而不是如何处理。(因为不是它处理的！)
对内调用 Exception Wrapper
对外调用 Public Exceptions

```
# 对内
wrapper = ExceptionWrapper.new(env, exception)

# 对外
config.exceptions_app || ActionDispatch::PublicExceptions.new(Rails.public_path)
response = @exceptions_app.call(env)
```

---

默认显示 public/ 目录下的错误页面(参考 PublicExceptions)。相关 HTTP response 是 X-Cascade 标识。

`config.exceptions_app` 可以配置，Show Excepting 抛异常时如何处理。如：

```ruby
# config/environments/production.rb
config.exceptions_app = ErrorController.action(:handle)
```

```ruby
# app/controllers/error_controller.rb
class ErrorController
  def handle
    flash.alert(t(".message"))
    redirect_to :back
  end
end
```

或者

```ruby
# config/application.rb
config.exceptions_app = self.routes
```

```ruby
# config/routes.rb
match '/404', via: :all, to: 'errors#not_found'
match '/422', via: :all, to: 'errors#unprocessable_entity'
match '/500', via: :all, to: 'errors#server_error'
```

```ruby
# app/controllers/errors_controller.rb
class ErrorsController < ActionController::Base
  layout 'error'

  def not_found
    render status: :not_found
  end

  def unprocessable_entity
    render status: :unprocessable_entity
  end

  def server_error
    render status: :server_error
  end
end
```

```
# app/views

errors/
  not_found.html.erb
  unprocessable_entity.html.erb
  server_error.html.erb
layouts/
  error.html.erb
```
