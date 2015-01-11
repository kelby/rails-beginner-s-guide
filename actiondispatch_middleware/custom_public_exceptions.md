#### 定制 Public Exceptions

默认的 Public Exceptions，显示 public/ 目录下的错误页面。

`config.exceptions_app` 可以配置 Show Excepting 抛异常时如何处理。如：

举例一：

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

举例二：

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

```ruby
# app/views

errors/
  not_found.html.erb
  unprocessable_entity.html.erb
  server_error.html.erb
layouts/
  error.html.erb
```
