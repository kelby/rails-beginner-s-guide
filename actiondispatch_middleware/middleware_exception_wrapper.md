**Exception Wrapper**

更友好的异常消息，包括可读性，回朔等。

被 Show Exceptions 调用。(另，它其实不是 middleware)

```ruby
ActionDispatch::ExceptionWrapper.rescue_responses

=> {"ActionController::RoutingError"=>:not_found,
  "AbstractController::ActionNotFound"=>:not_found,
  "ActionController::MethodNotAllowed"=>:method_not_allowed,
  "ActionController::NotImplemented"=>:not_implemented,
  "ActionController::InvalidAuthenticityToken"=>:unprocessable_entity,
  "ActiveRecord::RecordNotFound"=>:not_found,
  "ActiveRecord::StaleObjectError"=>:conflict,
  "ActiveRecord::RecordInvalid"=>:unprocessable_entity,
  "ActiveRecord::RecordNotSaved"=>:unprocessable_entity}
  
ActionDispatch::ExceptionWrapper.rescue_templates
=> {"ActionView::MissingTemplate"=>"missing_template",
  "ActionController::RoutingError"=>"routing_error",
  "AbstractController::ActionNotFound"=>"unknown_action",
  "ActionView::Template::Error"=>"template_error"}
```
