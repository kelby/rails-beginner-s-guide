## Parameters

**常用到的 params (也叫 parameters)等。**

```
parameters & params

path_parameters
```

在 new 页面，对 http://blog.test.example.com:3000/users 发起 POST 请求。

检测结果：

```ruby
request.parameters
=> {"utf8"=>"✓",
 "authenticity_token"=>"+.../hHj0C5Ao7CxXYwlejyghpNM3elUVw==",
 "user"=>{"name"=>"kelby", "email"=>""},
 "commit"=>"Create User",
 "controller"=>"users",
 "action"=>"create"}

# 另：
request.parameters == params
=> true
# 但它们两者并不是完全等价的

request.path_parameters
=> {:controller=>"users", :action=>"create"}
 ```
