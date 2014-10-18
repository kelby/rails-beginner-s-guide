# ActionDispatch Request

```
GET & query_parameters
POST & request_parameters

form_data?
delete?
get?
head?
key?
local?
patch?
post?
put?
xhr? & xml_http_request?

headers
ip
authorization
body
content_length
cookie_jar
deep_munge
flash
fullpath
media_type
method
method_symbol
original_fullpath
original_url
raw_post
remote_ip
request_method
request_method_symbol
server_software
uuid

check_path_parameters!

reset_session
session_options=

parse_query
```

## 获取当前绝对路径?

答：

使用 `request.original_url`
