## Request 文件下的内容

对 http://blog.test.example.com:3000/users?name=kelby 以起 GET 请求。

在 new 页面，对 http://blog.test.example.com:3000/users 以起 POST 请求。

**查询、请求参数**

```
GET & query_parameters
=> {"name"=>"kelby"}
=> {}

POST & request_parameters
=> {}
=> {"utf8"=>"✓",
    "authenticity_token"=>"+xxx.../yyy...==",
    "user"=>{"name"=>"kelby", "email"=>""},
    "commit"=>"Create User"}
```

**请求方法**

```
form_data?
=> false
=> true

delete?
=> false
=> false

get?
=> true
=> false

head?
=> false
=> false

patch?
=> false
=> false

post?
=> false
=> true

put?
=> false
=> false

xhr? & xml_http_request?
=> false
=> nil

local? # 来自本地请求？用正则判断，所以不是返回 true/false, 而是 0/nil.
=> 0
=> 0
```

**一些数据**

```
media_type
=> ""
=> "application/x-www-form-urlencoded"

method
=> "GET"
=> "POST"

method_symbol
=> :get
=> :post

raw_post
=> ""
=> "utf8=%E2%9C%93&authenticity_token=%...%...%3D%3D \n
    &user%5Bname%5D=kelby&user%5Bemail%5D=&commit=Create+User"

request_method
=> "GET"
=> "POST"

request_method_symbol
=> :get
=> :post

server_software
=> "thin"
=> "thin"

uuid
=> "6c1c3684-d8aa-4c03-97d3-d179997773ef"
=> "050daae4-c889-4d24-b627-ba682370216d"

ssl? # 来自于 Rack::Request, 当前是否在是用 https 加密协议。
=> false
=> false

scheme # 来自于 Rack::Request
=> "http"
=> "http"
```

**一些对象**

```
headers
=> ActionDispatch::Http::Headers 的实例对象(一大串东西)
=> ActionDispatch::Http::Headers 的实例对象(一大串东西)

authorization
=> nil
=> nil

body
=> StringIO 的实例对象
=> StringIO 的实例对象

content_length
=> 0
=> 187

cookie_jar
=> ActionDispatch::Cookies::CookieJar 的实例对象(一大串东西)
=> ActionDispatch::Cookies::CookieJar 的实例对象(一大串东西)

flash
=> ActionDispatch::Flash::FlashHash 的实例对象
=> ActionDispatch::Flash::FlashHash 的实例对象
```

```
check_path_parameters!
=> {:controller=>"users", :action=>"index"}
=> {:controller=>"users", :action=>"create"}
```

**客户端路径相关**

```
fullpath
=> "/users?name=kelby"
=> "/users"

original_fullpath
=> "/users?name=kelby"
=> "/users"

original_url
=> "http://blog.test.example.com:3000/users?name=kelby"
=> "http://blog.test.example.com:3000/users"
```

**服务端 IP 相关**

```
ip
=> "127.0.0.1"
=> "127.0.0.1"

remote_ip
=> "127.0.0.1"
=> "127.0.0.1"
```

**其它方法**

```
key?

deep_munge

reset_session
session_options=

parse_query
```

**除上述方法外，还有：**

```
include ActionDispatch::Http::Cache::Request
include ActionDispatch::Http::MimeNegotiation
include ActionDispatch::Http::Parameters
include ActionDispatch::Http::FilterParameters
include ActionDispatch::Http::URL
```
