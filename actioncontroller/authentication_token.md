## Token

headers["WWW-Authenticate"] = %(Token realm="#{realm.gsub(/"/, "")}")

提供方法：

```
authenticate
authentication_request

encode_credentials

params_array_from

rewrite_param_values

token_and_options

raw_params
token_params_from
```

传递 token 时，可以传递额外的数据，如：

```
Authorization: Token token="abc", nonce="def"
```

然后使用 `token_and_options` 获取这些数据。

使用举例：

```ruby
def test_access_granted_from_xml
  get(
    "/notes/1.xml", nil,
    'HTTP_AUTHORIZATION' =>
     ActionController::HttpAuthentication::Token.encode_credentials(users(:dhh).token)
  )

  assert_equal 200, status
end
```

### Controller Methods

提供方法：

```
authenticate_with_http_token
request_http_token_authentication

authenticate_or_request_with_http_token
```

使用举例：

```ruby
class PostsController < ApplicationController
  TOKEN = "secret"

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
        if user = authenticate_with_http_token do |t, o|
          @account.users.authenticate(t, o)
          end

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

**Token 验证的部分特点**

1. 不能直接明文出现在 url 里。
2. 通过 curl -H 'Authorization: Token token="x"' 传递数据。
3. 通常用于制作程序接口。
4. 不提供弹窗输入，其余两种验证方式提供。
