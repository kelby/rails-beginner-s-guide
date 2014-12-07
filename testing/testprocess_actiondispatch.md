## Action Dispatch

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

### Test Process

```
assigns
cookies
flash
session

fixture_file_upload
redirect_to_url
```

### Test Request

```
accept=
action=

cookies & rack_cookies

host=

if_modified_since=
if_none_match=

path=
port=

remote_addr=
request_method=
request_uri=

user_agent=
```

### Test Response

```
from_response

# response successful?
success? & successful?

# URL not found?
missing? & not_found?

# redirected?
redirect? & redirection?

# a server-side error?
error? & server_error?
```

### Integration Test

**类方法**

```
app
```

**实例方法**

```
app
app=

document_root_element

url_options
```

**Request Helpers**

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
