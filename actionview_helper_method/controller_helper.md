## Controller Helper

```ruby
delegate :request_forgery_protection_token, :params, :session, :cookies, :response, :headers,
         :flash, :action_name, :controller_name, :controller_path, :to => :controller
```
