## Http Authentication

3 个级别的验证，分别是 Basic、Digest 和 Token.

## Basic

提供方法：

```
auth_param, auth_scheme, authenticate, authentication_request
decode_credentials
encode_credentials
user_name_and_password

has_basic_credentials?
```

使用举例：

```ruby
def test_access_granted_from_xml
  get(
    "/notes/1.xml", nil,
    'HTTP_AUTHORIZATION' => ActionController::HttpAuthentication::Basic.encode_credentials(users(:dhh).name, users(:dhh).password)
  )

  assert_equal 200, status
end
```

### ControllerMethods

提供方法：

```
authenticate_or_request_with_http_basic
authenticate_with_http_basic
request_http_basic_authentication
```

使用举例：

```ruby
class ApplicationController < ActionController::Base
  before_action :set_account, :authenticate

  protected
    def set_account
      @account = Account.find_by(url_name: request.subdomains.first)
    end

    def authenticate
      case request.format
      when Mime::XML, Mime::ATOM
        # 使用验证
        if user = authenticate_with_http_basic { |u, p| @account.users.authenticate(u, p) }
          @current_user = user
        else
          # 使用验证
          request_http_basic_authentication
        end
      else
        if session_authenticated?
          @current_user = @account.users.find(session[:authenticated][:user_id])
        else
          redirect_to(login_url) and return false
        end
      end
    end
end
```

### ClassMethods

提供方法：

```
http_basic_authenticate_with
```

使用举例：

```ruby
class PostsController < ApplicationController
  http_basic_authenticate_with name: "dhh", password: "secret", except: :index

  def index
    render plain: "Everyone can see me!"
  end

  def edit
    render plain: "I'm only accessible if you know the password"
  end
end
```

> Note: Basic 提供的 authenticate_or_request_with_http_basic 应该是最常用的验证方式了吧。

## Digest

提供方法：

```
authenticate, authentication_header, authentication_request
decode_credentials, decode_credentials_header
encode_credentials, expected_response
ha1
nonce
opaque
secret_token
validate_digest_response, validate_nonce
```

### ControllerMethods

提供方法：

```
authenticate_or_request_with_http_digest
authenticate_with_http_digest
request_http_digest_authentication
```

使用举例：

```ruby
require 'digest/md5'
class PostsController < ApplicationController
  REALM = "SuperSecret"
  USERS = {"dhh" => "secret", #plain text password
           "dap" => Digest::MD5.hexdigest(["dap",REALM,"secret"].join(":"))}  #ha1 digest password

  before_action :authenticate, except: [:index]

  def index
    render plain: "Everyone can see me!"
  end

  def edit
    render plain: "I'm only accessible if you know the password"
  end

  private
    def authenticate
      # 使用验证
      authenticate_or_request_with_http_digest(REALM) do |username|
        USERS[username]
      end
    end
end
```

## Token

提供方法：

```
authenticate, authentication_request
encode_credentials
params_array_from
raw_params, rewrite_param_values
token_and_options, token_params_from
```

使用举例：

```ruby
def test_access_granted_from_xml
  get(
    "/notes/1.xml", nil,
    'HTTP_AUTHORIZATION' => ActionController::HttpAuthentication::Token.encode_credentials(users(:dhh).token)
  )

  assert_equal 200, status
end
```

### ControllerMethods

提供方法：

```
authenticate_or_request_with_http_token
authenticate_with_http_token
request_http_token_authentication
```

使用举例：

```ruby
class PostsController < ApplicationController
  TOKEN = "secret"

  before_action :authenticate, except: [ :index ]

  def index
    render plain: "Everyone can see me!"
  end

  def edit
    render plain: "I'm only accessible if you know the password"
  end

  private
    def authenticate
      # 使用验证
      authenticate_or_request_with_http_token do |token, options|
        token == TOKEN
      end
    end
end
```

再次举例：

```ruby
class ApplicationController < ActionController::Base
  before_action :set_account, :authenticate

  protected
    def set_account
      @account = Account.find_by(url_name: request.subdomains.first)
    end

    def authenticate
      case request.format
      when Mime::XML, Mime::ATOM
        # 使用验证
        if user = authenticate_with_http_token { |t, o| @account.users.authenticate(t, o) }
          @current_user = user
        else
          # 使用验证
          request_http_token_authentication
        end
      else
        if session_authenticated?
          @current_user = @account.users.find(session[:authenticated][:user_id])
        else
          redirect_to(login_url) and return false
        end
      end
    end
end
```
