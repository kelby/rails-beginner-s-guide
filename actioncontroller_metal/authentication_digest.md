## Digest

对应头部字段及内容：

```
headers["WWW-Authenticate"] = %(Digest realm="#{realm}", qop="auth", algorithm=MD5, nonce="#{nonce}", opaque="#{opaque}")
```

**Controller 方法：**

提供方法：

```
authenticate_with_http_digest
request_http_digest_authentication

authenticate_or_request_with_http_digest
```

使用举例：

```ruby
require 'digest/md5'
class PostsController < ApplicationController
  REALM = "SuperSecret"
  USERS = {"dhh" => "secret",
           # plain text password
           # ha1 digest password
           "dap" => Digest::MD5.hexdigest(["dap", REALM, "secret"].join(":"))}

  before_action :authenticate, except: :index

  def index
    render plain: "Everyone can see me!"
  end

  def edit
    render plain: "I'm only accessible if you know the password"
  end

  private
    def authenticate
      # 使用验证
      authenticate_or_request_with_http_digest(REALM) do |username| # <- 这里
        USERS[username]
      end
    end
end
```

`authenticate_or_request_with_http_digest` 简单的封装了其余两个方法。  
从名字可知，如果提供的是普通文本则直接接受；如果提供的是 md5 加密，则先(自动)解密再接受。

**其它方法：**

```
authenticate
authentication_header
authentication_request

decode_credentials
decode_credentials_header
encode_credentials

expected_response
ha1
nonce
opaque
secret_token

validate_digest_response
validate_nonce
```
