## Rack - Ruby Web server 接口

用 Ruby 或"Ruby 实现的 Web 框架"创建的 app应用，想要提供 Web 服务，它们都要和 Web服务器打交道。而这个过程是非常复杂的，为了简化这个过程，我们可以使用 Rack.

> app应用 --> (Rack) --> 应用服务器 --> Web服务器 --> 外部世界

Rack 提供了一个"与 Web服务器打交道"最精简的接口，通过这个接口，我们的应用很轻松的就能提供 Web 服务(接收 Web 请求，响应处理结果)。上面的"应用服务器"是对 Rack 的进一步封装。

使用这个接口的条件是：传递一个"程序"(你没看错，就是把一个程序当做参数，下文以 app 代替)。并且这个 app 需要同时满足以下条件：

- app.respond_to? :call # => true
- app 创建过程需要以运行环境做为参数(类型为 Hash，下文以 env 代替运行环境)
- app 需要返回一个数组，数组包含 3 个元素，依次是：

  - [HTTP status code](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
  - [HTTP headers](http://en.wikipedia.org/wiki/List_of_HTTP_headers) (类型为 Hash)
  - [HTTP body data](http://en.wikipedia.org/wiki/HTTP_body_data) (类型能接受 `each` 方法即可)

举个例子:

```ruby
# my_rack_app.rb

require 'rack'

app = Proc.new do |env|
  ['200', {'Content-Type' => 'text/html'}, ['A example rack app.']]
end

Rack::Handler::WEBrick.run app
```

你也可以使用 `rackup` 命令，节省点时间和力气：

```ruby
# config.ru

run Proc.new { |env| ['200', {'Content-Type' => 'text/html'}, ['get rack\'d']] }
```

运行:

`rackup config.ru`

... 完。

---

可以封装 Rack，得到我们自己的 YourRack，或

```ruby
class YourRack
  def initialize(app)
    @app = app
  end

  def initialize(app)
    @app = app
  end

  def call(env)
    @app.call(env)
  end
end
```

### 使用举例

'rack/contrib' 可以自动加载 Rack 包含的所有组件，以下例子可用 `rackup config.ru` 运行：

```ruby
require 'rack'
require 'rack/contrib'

use Rack::Profiler if ENV['RACK_ENV'] == 'development'

use Rack::ETag
use Rack::MailExceptions

run theapp
```

链接

[Rack: a Ruby Webserver Interface](http://rack.github.io/)<br>
[a modular Ruby webserver interface](https://github.com/rack/rack)<br>
[Contributed Rack Middleware and Utilities](https://github.com/rack/rack-contrib)
