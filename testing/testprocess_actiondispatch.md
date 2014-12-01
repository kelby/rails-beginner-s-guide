## Action Dispatch

### Test Process

```
assigns
cookies
flash
session

fixture_file_upload
redirect_to_url
```

### Assertions

```
html_document
```

**Response Assertions**

```
assert_redirected_to
assert_response
```

**Routing Assertions**

```
assert_generates
assert_recognizes
assert_routing

with_routing
```

### Test Request

```
accept=
action=

cookies

host=

if_modified_since=
if_none_match=

path=
port=

rack_cookies
remote_addr=
request_method=
request_uri=

user_agent=
```

### Test Response

```
from_response

# Was the response successful?
success? & successful?

# Was the URL not found?
missing? & not_found?

# Were we redirected?
redirect? & redirection?

# Was there a server-side error?
error? & server_error?
```

### Integration Test

```
app
app=

document_root_element

url_options
```

`app` 包含了类方法 & 实例方法

**RequestHelpers**

```
delete

delete_via_redirect

follow_redirect!

get

get_via_redirect

head

patch

patch_via_redirect

post

post_via_redirect

put

put_via_redirect

request_via_redirect

xhr & xml_http_request
```

**Session**

```
cookies

host

https?
https!

reset!

url_options
```

**Runner**

```
app

default_url_options
default_url_options=

open_session

reset!
```
