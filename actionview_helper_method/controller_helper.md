### Controller Helper

**委托 Controller 方法给 View 使用。**

```ruby
delegate :params, :session, :cookies, :flash,
         :response, :headers, :action_name, :controller_name, :controller_path,
         :request_forgery_protection_token,
         :to => :controller
```

效果和使用 `helper_method` 类似。

ActionView::Base initialize 时就会引入。(其实，它就是从 Base 里抽取出来的)
