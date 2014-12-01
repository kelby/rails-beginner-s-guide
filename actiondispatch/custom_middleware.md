## 定制 Middleware

每一个 middleware 本质就是一个 Rack app.

Middleware 处理的是 @app 和 env，功能有限，并且仍然属于底层实现。

**举例一**

放在 app/middleware/ 目录下，按照 middleware 写。然后在 config 里 use 就行了，不用做其它的配置。

```ruby
class Scrubber
  def initialize(app, options)
    @app = app
    @routes = options[:routes]
  end

  def call(env)
    scrub(env)
    @app.call(env)
  end

  private
    def scrub(env)
      return unless @routes.include?(env["PATH_INFO"])
      rack_input = env["rack.input"].read
      params = Rack::Utils.parse_query(rack_input, "&")
      params["xml"] = Rack::Utils.unescape(params["xml"])
      env["rack.input"] = StringIO.new(Rack::Utils.build_query(params))
    rescue
    ensure
      env["rack.input"].rewind
    end
end
```

然后

```ruby
# 注意，可直接引用相关类
config.middleware.insert_before ActionController::ParamsParser,
                             "Scrubber",
                             :routes => [ "/examples/scrubbed" ]
```

**举例二**

放在 lib/ 目录下，按照 middleware 写。然后在 config 里 use 就行了，不用做其它的配置。

```ruby
class ResponseTimer
  def initialize(app, message = "Response Time")
    @app = app
    @message = message
  end

  def call(env)
    dup._call(env)
  end

  def _call(env)
    @start = Time.now
    @status, @headers, @response = @app.call(env)
    @stop = Time.now
    [@status, @headers, self]
  end

  def each(&block)
    block.call("<!-- #{@message}: #{@stop - @start} -->\n") if @headers["Content-Type"].include? "text/html"
    @response.each(&block)
  end
end
```

然后

```ruby
# 注意，需以字符串的方式引用
config.middleware.use "ResponseTimer", "Load Time"
```

**参考**

[Rails on Rack](http://guides.rubyonrails.org/rails_on_rack.html)

[Where do you put your Rack middleware files and requires?](http://stackoverflow.com/questions/3428343/where-do-you-put-your-rack-middleware-files-and-requires)

[Sanitizing POST params with custom Rack middleware](http://pivotallabs.com/sanitizing-post-params-with-custom-rack-middleware/)

[#151 Rack Middleware](http://railscasts.com/episodes/151-rack-middleware)
