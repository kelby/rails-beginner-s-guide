# ActionDispatch Http

和 Web 服务器已经很接近了，接收的是 request, 响应的是 response.

```ruby
request =>
  @env = { key1: "value1", key2: value2", ... }
  @filtered_env=nil,
  @filtered_parameters=nil,
  @filtered_path=nil,
  @fullpath=nil,
  @ip=nil,
  @method=nil,
  @original_fullpath=nil,
  @port=nil,
  @protocol=nil,
  @remote_ip=nil,
  @request_method=nil,
  @symbolized_path_params=nil,
  @uuid=nil>
```

**URL**

```
domain
extract_domain, extract_subdomain, extract_subdomains
full_url_for
host, host_with_port
optional_port
path_for, port, port_string, protocol
raw_host_with_port
server_port, standard_port, standard_port?, subdomain, subdomains
url, url_for
```

**UploadedFile**

```
close
eof?
open
path
read, rewind
size
```

**Response**

```
_status_code
abort, await_commit, await_sent
body, body=, body_parts
close, code, commit!, committed?, content_type=, cookies
delete_cookie
location, location=
message
prepare!
redirect_url, response_code
sending!, sending?, sent!, sent?, set_cookie, status=, status_message
to_a, to_ary
```

**Request**

```
GET
POST
authorization
body
check_path_parameters!, content_length, cookie_jar
deep_munge, delete?
flash, form_data?, fullpath
get?
head?, headers
ip
key?
local?
media_type, method, method_symbol
original_fullpath, original_url
parse_query, patch?, post?, put?
query_parameters
raw_post, remote_ip, request_method, request_method_symbol, request_parameters, reset_session
server_software, session_options=
uuid
xhr?, xml_http_request?
```

**RailsMetaStore && RailsEntityStore**

都没有任何文档说明的。

**Parameters**

```
parameters & params

path_parameters
symbolized_path_parameters
```

**Mimes && Type**

**MimeNegotiation**

```
accepts
content_mime_type, content_type
format, formats
negotiate_mime

format=
formats=
variant=

use_accept_header
valid_accept_header
```

**Headers**

```
[], []=
each
fetch
include?
key?
merge, merge!
```

**FilterRedirect**

```
filtered_location
```

**FilterParameters && ParameterFilter**

```
env_filter
filtered_env, filtered_parameters, filtered_path, filtered_query_string
parameter_filter_for

parameter_filter
```

原理类似：

```ruby
env["action_dispatch.parameter_filter"] = [:password]
=> replaces the value to all keys matching /password/i with "[FILTERED]"

env["action_dispatch.parameter_filter"] = [:foo, "bar"]
=> replaces the value to all keys matching /foo|bar/i with "[FILTERED]"

env["action_dispatch.parameter_filter"] = lambda do |k,v|
  v.reverse! if k =~ /secret/i
end
=> reverses the value to all keys matching /secret/i
```

> Note: 用得比较多的是方法 parameter_filter，通常用来配置过滤 :password ... 过滤的其实是 ParameterFilter 的实例对象，虽然这里没有体现出来。

**Cache**

### Request 

```
etag_matches?
fresh?
if_modified_since, if_none_match, if_none_match_etags
not_modified?
```

###  Response

```
date, date=, date?
etag=
last_modified, last_modified=, last_modified?
```
