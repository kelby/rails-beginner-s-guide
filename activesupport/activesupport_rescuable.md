## Rescuable

`rescue_from(*klasses, &block)`

klasses 表示一个或多个异常类。如果有 :with 选项，则用其 value(一般是个方法) 处理；否则，需要传递 block 来处理。

```ruby
class ApplicationController < ActionController::Base
  # 一个或多个异常类，有 :with 选项
  rescue_from User::NotAuthorized, with: :deny_access # self defined exception
  rescue_from ActiveRecord::RecordInvalid, with: :show_errors

  # 一个或多个异常类，传递 block
  rescue_from 'MyAppError::Base' do |exception|
    render xml: exception, status: 500
  end

  protected
    def deny_access
      ...
    end

    def show_errors(exception)
      exception.record.new_record? ? ...
    end
end
```

实现：类似'实例变量的运用'，`rescue_from` 把希望捕捉的异常类放到一个变量(rescue_handlers)里。抛出异常时，会对异常进行检查(rescue_with_handler)，如果抛出的异常恰好被包含在这个变量(rescue_handlers)，则用我们的方式进行处理。
