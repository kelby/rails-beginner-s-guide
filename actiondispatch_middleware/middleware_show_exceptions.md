### Show Exceptions

程序抛异常时，用什么程序来处理。

它只是接口，它关心的是用什么，而不是如何处理。(因为不是它处理的！)
对内调用 Exception Wrapper
对外调用 Public Exceptions.

```
# 对内
wrapper = ExceptionWrapper.new(env, exception)

# 对外
config.exceptions_app || ActionDispatch::PublicExceptions.new(Rails.public_path)
response = @exceptions_app.call(env)
```
