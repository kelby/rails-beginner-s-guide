## Basic

对应头部字段及内容：

```ruby
headers["WWW-Authenticate"] = %(Basic realm="#{realm.gsub(/"/, "")}")
```

**类方法：**

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

`http_basic_authenticate_with` 除 :name 和 :password 选项外，一般还可设置 :realm 做为提示信息。它已经封装了 authenticate_or_request_with_http_basic 方法。

`http_basic_authenticate_with` 最常用的验证方式。

**Controller 方法：**

```
authenticate_with_http_basic
request_http_basic_authentication

authenticate_or_request_with_http_basic
```

`authenticate_or_request_with_http_basic` 简单的封装了其余两个方法。

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
        if user = authenticate_with_http_basic do |u, p|
            @account.users.authenticate(u, p) # <- 这里
          end

          @current_user = user
        else
          # 使用验证
          request_http_basic_authentication # <- 这里
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

**其它方法：**

```
auth_param
auth_scheme

authenticate

authentication_request

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
    'HTTP_AUTHORIZATION' =>
     ActionController::HttpAuthentication::Basic.encode_credentials(
       users(:dhh).name,
       users(:dhh).password
     )
  )

  assert_equal 200, status
end
```
