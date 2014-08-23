Controller 里的 public方法(也就是action) 会自动对应 Route 里的路由规则。当请求到来时，action接受请求并处理，最后渲染相应视图模板(Get-and-show)或重定向到另一action(do-and-redirect)。

默认，只有 ApplicationController 直接继承于 ActionController::Base，其它的控制器继承于 ApplicationController。所以，如果你想在所有controller处理之前做一些什么，你可以把它们写在 ApplicationController 里。

## Metal

**ActionController::Metal** is the simplest possible controller, providing a valid Rack interface without the additional niceties provided by **ActionController::Base**

If you have a Rails application that has service end points that need to be really, really fast. So fast that the few milliseconds that a trip through the Rails router and Action Controller path is too much.

For this scenario, we’ve built a thin wrapper around the generic Rack middleware and given it a place in the hierarchy along with the name “Metal”.

> Rails Metal is a subset of Rack middleware

下面对 metal/ 目录下面包含的东西，做一下简述：

### MimeResponds & Responder

in class
`respond_to(*mimes)` - 类型(配合 in action respond_with)，可加 :only 或 :except 指定 actions

in action
`respond_to(*mimes, &block)` - 全部内容(包含类型和内容)
`respond_with(*resources, &block)` - 内容(配合 in class respond_to)


Defines mime types that are rendered by default when invoking respond_with.

### Redirecting

```ruby
redirect_to(options = {}, response_status = {})
```

是重要的部分就是根据不同的 options，计算出 url 路径：

```ruby
# 计算出 url 路径
def _compute_redirect_to_location(options) #:nodoc:
  case options
  # The scheme name consist of a letter followed by any combination of
  # letters, digits, and the plus ("+"), period ("."), or hyphen ("-")
  # characters; and is terminated by a colon (":").
  # See http://tools.ietf.org/html/rfc3986#section-3.1
  # The protocol relative scheme starts with a double slash "//".
  when /\A([a-z][a-z\d\-+\.]*:|\/\/).*/i
    options
  when String
    request.protocol + request.host_with_port + options
  when :back
    request.headers["Referer"] or raise RedirectBackError
  when Proc
    _compute_redirect_to_location options.call
  else
    url_for(options)
  end.delete("\0\r\n")
end
```

根据不同的 options，计算出 url 路径，使用举例：

```ruby
# String
redirect_to “www.rubyonrails.org”
redirect_to “/images/screenshot.jpg”
redirect_to articles_url

# :back
redirect_to :back

# Proc
redirect_to proc { edit_post_url(@post) }

# 其它举例(由 url_for 处理)：

# - Record
redirect_to post

# - Hash
redirect_to({ action: 'atom' }, alert: "Something serious happened")
redirect_to action: “show”, id: 5
```

相关、类似功能：

`url_for` Generate a url based on the options provided, default_url_options and the routes defined in routes.rb.

`polymorphic_url` Constructs a call to a named RESTful route for the given record and returns the resulting URL string。

极端情况下，会发生 redirect_to -> url_for -> ActionController::UrlFor -> AbstractController::UrlFor -> ActionDispatch::Routing::UrlFor --> ActionDispatch::Routing::PolymorphicRoutes -> polymorphic_url

### StrongParameters & Parameters

我们常用到 StrongParameters 提供的下列方法：

```ruby
params()
params=(value)
```

但它封装的却是 Parameters 的实例对象，所以它有下列方法：

```ruby
permitted?
permit!
require(key) - alias :required :require
permit(*filters)

[](key)
fetch(key, *args)
slice(*keys)
dup
```

params 已经定义了，内容从 request 里来：

```ruby
params == request.parameters
=> true
```

这个对象的值是什么？- 表单数据或传递过来的，加上 Controller 和 action

```ruby
Processing by PostsController#create as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=", "post"=>{"title"=>"hello world"}, "commit"=>"Create Post"}

params
=> {"utf8"=>"✓",
 "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=",
 "post"=>{"title"=>"hello world"},
 "commit"=>"Create Post",
 "action"=>"create",
 "controller"=>"posts"}
```

我们也可以在 Rails之外 创建自己的 params

```ruby
require 'action_controller/parameters'

params = ActionController::Parameters.new({
  person: {
    name: 'Francesco',
    age:  22,
    role: 'admin'
  }
})

# require(key) 和 permit(*filters) 方法
permitted = params.require(:person).permit(:name, :age)
permitted            # => {"name"=>"Francesco", "age"=>22}
permitted.class      # => ActionController::Parameters
# permitted? 方法
permitted.permitted? # => true

Person.first.update!(permitted)
# => #<Person id: 1, name: "Francesco", age: 22, role: "user">
```

## Caching::Fragments

Caching is a cheap way of speeding up slow applications by keeping the result of calculations, renderings, and database calls around for subsequent requests.

You can read more about each approach by clicking the modules below.

要使用，先配置。Note: To turn off all caching, set

```ruby
config.action_controller.perform_caching = false
```

**Caching stores**

All the caching stores from ActiveSupport::Cache are available to be used as backends for Action Controller caching.

Configuration examples (MemoryStore is the default):

```ruby
config.action_controller.cache_store = :memory_store
config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
config.action_controller.cache_store = :mem_cache_store, 'localhost'
config.action_controller.cache_store = :mem_cache_store, Memcached::Rails.new('localhost:11211')
config.action_controller.cache_store = MyOwnStore.new('parameter')
```

页面缓存、action缓存都被干掉了，留下很好的片段缓存。

视图包含两类内容：Controller传递过来的实例变量(动态内容，必需手动设置)，本身的静态内容(自动设置)。
缓存要考虑这两种内容的更新。

还有就是视图里的显示是层层嵌套的，它们之间有时候是有关联的，这种情况也要考虑。

```
# 第一次访问
  Cache digest for app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197 (0.1ms)
Write fragment views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197 (1.1ms)

# 第二次访问
  Cache digest for app/views/posts/show.html.erb: a357e54a8e1fdeff463f2da17cdc8197
Read fragment views/posts/1-20140421062215459004000/a357e54a8e1fdeff463f2da17cdc8197 (0.6ms)

# post.cache_key
 => "posts/1-20140421062215459004000"

# 更改静态内容(默认)
  Cache digest for app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc (0.1ms)
Write fragment views/posts/1-20140421062215459004000/6e30019bd1127688840f7307cbe5cfbc (1.4ms)

# 更改动态内容(在这里是update post)
  Cache digest for app/views/posts/show.html.erb: 6e30019bd1127688840f7307cbe5cfbc
Read fragment views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc (0.1ms)
Write fragment views/posts/1-20140421064029939882000/6e30019bd1127688840f7307cbe5cfbc (1.1ms)

Read fragment views/localhost:3000/posts/1?action_suffix=post1/1a3c7591dece4354ee7da69dfc12f246 (0.2ms)
Write fragment views/localhost:3000/posts/1?action_suffix=post1/1a3c7591dece4354ee7da69dfc12f246 (9.0ms)

怎么生成的？
views/projects/123-20120806214154/7a1156131a6928cb0026877f8b749ac9
      ^class   ^id ^updated_at    ^template tree digest
```

如何才能完全手动管理缓存？

```ruby
def fragment_name_with_digest(name) #:nodoc:
  names  = Array(name.is_a?(Hash) ? controller.url_for(name).split("://").last : name)
  digest = Digestor.digest name: @virtual_path, finder: lookup_context, dependencies: view_cache_dependencies

  [ *names, digest ]
end
```

1. 不要使用动态内容做 key
2. 关闭默认的加密

cache 'all_available_products', skip_digest: true

expire_fragment('all_available_products') 才有用。

http://rubyer.me/blog/2012/09/04/speed-up-with-rails-cache/
http://blog.xdite.net/posts/2012/09/02/cache-digest-new-strategy/
http://hawkins.io/2012/07/advanced_caching_part_2-using_strategies/
http://www.codelearn.org/blog/rails-cache-with-examples

> **Note:** 在 Controller 和 View 里写缓存相关的代码，这真的很丑陋。并且一个页面往往由很多元素组成，只能在它的 action 里管理显然很不方便。推荐 cells

## 其它
------

### RequestForgeryProtection

```ruby
protect_from_forgery(options = {})
```

### HttpAuthentication

有3类：Basic，Digest，Token

我们只看一个最最常用的：

```ruby
authenticate_or_request_with_http_basic(realm = "Application", &login_procedure)
```

### ConditionalGet

Rails 3.1 Moved etag responsibility from ActionDispatch::Response to the middleware stack.

ETag 方面的知识，基于资源的HTTP Cache的实现介绍。感兴趣可以查看 [在你的 Rails App 中开启 ETag 加速页面载入同时节省资源](http://huacnlee.com/blog/use-etag-in-your-rails-app-to-speed-up-loading/) ，我是认为不是很必要。缓存方案那么多。

```ruby
# 单独使用没意义
etag(&etagger) - 追加元素到 etag

# 决定是否改变。Rails 4 以后会对页面内容进行 MD5 求值，fresh_when 设置不合理的话，会得到旧数据。
fresh_when(record_or_options, additional_options = {}) - 满足etag刷新条件，才执行后面操作

# 还有
expires_in(seconds, options = {})
expires_now()
stale?(record_or_options, additional_options = {})
```

`fresh_when` 比较关键，决定是 ETag 和 last_modify
`etag` 是类方法，就相当于一个过滤器。把元素加入到每一个 fresh_when 的 record_or_options 里。

```
X-Runtime	219
Date	Fri, 02 May 2014 13:39:45 GMT
Content-Encoding	gzip
Vary	Accept-Encoding
Server	ITeye SERVER

ETag	"a6ae1d7498b3a3963e7af3ff57e7f3ec"

Transfer-Encoding	chunked
Content-Type	text/html; charset=utf-8
Cache-Control	private, max-age=0, must-revalidate
```

Supporting the etag and last modified timestamp in HTTP headers means that Rails can now send back an empty response if it gets a request for a resource that hasn't been modified lately. This allows you to check whether a response needs to be sent at all.

和所有缓存一样，怎么生成 cache_key ？

最新，表示没有更改。

> **Note:** 注意 ETag 和已经被废除 cache_page 的区别，前者是Web服务器级别，后者是应用服务器级别。如果页面没有更改，ETag 返回的是 "304 Not Modified"，其他什么都不用干，连网络带宽都省了。而 cache_page 还要读取 public/ 目录下的静态HTML文件。

什么也没改 f9d7568ac634fc9ad270f2348d5f3b41
更改表态内容 d9cc40e9e225a3e738c68b01f17ef4b0
update_columns 7128cbb34d84aa096ee62d69d1599a32
update_attributes 98eac754b15c61f4c8b4f79b7f0a5645

> **Note:** 默认 动态或静态内容有变，ETag 都会更新；都不改变，才不更新。如果手动设置了 fresh_when 反而会获取到旧数据。

### Flash

声明后，在 redirect_to 等场合，可直接用被声明的 flash 类型；在 View 里直接显示。

`add_flash_types(*types)`

```ruby
# in application_controller.rb
class ApplicationController < ActionController::Base
  add_flash_types :warning
end

# in your controller
redirect_to user_path(@user), warning: "Incomplete profile"
# 打开了原有的 redirect_to 方法

# in your view
<%= warning %>
# 对应着：
define_method(type) do
  request.flash[type]
end
helper_method type
```

很简单的一方法，见 [Rails edge 支援 add_flash_types](http://ruby-china.org/topics/4207)

### Streaming & Live

Rails 默认的渲染过程先是模板，然后才是布局。完成模板，及需要的查询，最后才完成布局。

Streaming 可以改变一下顺序，按布局来渲染。布局先显示，对于用户体验可能更好一点，还有就是这会使得JS和CSS的加载顺序比平时提前。

使用很简单，在 `render` 的时候，加上 `:stream` 参数即可：

```ruby
class PostsController
  def index
    @posts = Post.all
    render stream: true
  end
end
```

Live 用于构建实时聊天之类的。

### ParamsWrapper

我们用 POST 请求数据时，可以看到在 params 里对象封装在一个 root 元素里，例如：

```
"post"=>{"title"=>"hello world"}
```

我们获取里面的某个属性，只能这样

```ruby
params[:post][:title]

# 或

post = Post.new(params[:post])
post.title
```

在不引起误解的情况下，我们希望是能够通过 `params[:title]` 直接获取这个元素。当然这使用场景很有限。

```ruby
wrap_parameters format: :xml
  # enables the parameter wrapper for XML format

wrap_parameters :person
  # wraps parameters into +params[:person]+ hash

wrap_parameters Person
  # wraps parameters by determining the wrapper key from Person class
  (+person+, in this case) and the list of attribute names

wrap_parameters include: [:username, :title]
  # wraps only +:username+ and +:title+ attributes from parameters.

wrap_parameters false
  # disables parameters wrapping for this controller altogether.
```

`include_root_in_json` 一个功能上恰好和它相反的配置方法(此外，一个在 ActiveModel，另一个在 ActionController)。

```ruby
Post.to_json, you'll get:

# 默认，没有 root 元素
{title: 'hello world'}

# 如果 include_root_in_json = true
{"post": {title: 'hello world'}}
```

## Others

ActionController include 了这些模块，而我们自定义的 Controller 又继承于 ActionController.

自然的，它们的 ClassMethods 就会变成我们自定义 Controller 的类方法，而其它方法则类似实例方法，可运用于 action.

## 参考

[Introducing Rails Metal](http://weblog.rubyonrails.org/2008/12/17/introducing-rails-metal/)
[Rails on Rack](http://edgeguides.rubyonrails.org/rails_on_rack.html)

[Developing api with rails metal](http://www.slideshare.net/artellectual/developing-api-with-rails-metal)
[Rails Metal](http://railscasts.com/episodes/150-rails-metal)
[基于资源的HTTP Cache的实现介绍](http://robbinfan.com/blog/13/http-cache-implement)
