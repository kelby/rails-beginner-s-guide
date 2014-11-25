## Request Forgery Protection

```
compare_with_real_token
form_authenticity_param, form_authenticity_token
handle_unverified_request
masked_authenticity_token
real_csrf_token
verify_authenticity_token, verify_same_origin_request
xor_byte_strings

mark_for_same_origin_verification!

marked_for_same_origin_verification?
non_xhr_javascript_response?
protect_against_forgery?
valid_authenticity_token?, verified_request?

```

## Class Methods

```
protect_from_forgery
```

使用举例：

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery
  skip_before_action :verify_authenticity_token, if: :json_request?

  protected

  def json_request?
    request.format.json?
  end
end
```
