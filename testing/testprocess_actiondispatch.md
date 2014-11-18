## ActionDispatch

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
delete, delete_via_redirect
F
follow_redirect!
G
get, get_via_redirect
H
head
P
patch, patch_via_redirect, post, post_via_redirect, put, put_via_redirect
R
request_via_redirect
X
xhr, xml_http_request
```

**Session**

```
cookies
H
host, https!, https?
N
new
R
reset!
U
url_options
```

**Runner**

```
app
D
default_url_options, default_url_options=
M
method_missing
O
open_session
R
reset!, respond_to?
```
