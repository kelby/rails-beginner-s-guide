### rspec rails 示例

**Routing specs**

```
expect(:get => "/articles/2012/11/when-to-use-routing-specs").to route_to(
  :controller => "articles",
  :month => "2012-11",
  :slug => "when-to-use-routing-specs"
)
```

```
expect(:delete => "/accounts/37").not_to be_routable
```

**Controller specs**

```ruby
expect(response).to render_template(:new)   # wraps assert_template

expect(response).to redirect_to(location)   # wraps assert_redirected_to

expect(response).to have_http_status(:created)

expect(assigns(:widget)).to be_a_new(Widget)

render_views

bypass_rescue
```

**Bypass rescue**

背景：

```ruby
class AccessDenied < StandardError; end

class ApplicationController < ActionController::Base
  rescue_from AccessDenied, :with => :access_denied

  private

  def access_denied
    redirect_to "/401.html"
  end
end
```

默认(rescue_from 起作用)：

```ruby
require 'controllers/gadgets_controller_spec_context'

RSpec.describe GadgetsController do
  before do
    def controller.index
      raise AccessDenied
    end
  end

  describe "index" do
    it "redirects to the /401.html page" do
      get :index
      expect(response).to redirect_to("/401.html")
    end
  end
end
```

使用 bypass_rescue

```ruby
require 'controllers/gadgets_controller_spec_context'

RSpec.describe GadgetsController do
  before do
    def controller.index
      raise AccessDenied
    end
  end

  describe "index" do
    it "raises AccessDenied" do
      bypass_rescue
      expect { get :index }.to raise_error(AccessDenied)
    end
  end
end
```

链接 [Bypass rescue](https://relishapp.com/rspec/rspec-rails/docs/controller-specs/bypass-rescue)
