# ActionController 源码剖析

Railties#Helpers

给我们定义的 Controller 加载所有可用的 helpers.

```ruby
if klass.superclass == ActionController::Base && ActionController::Base.include_all_helpers
  klass.helper :all
end
```
