## Rails assets precompile

### 概述

**Assets Pipeline 主要功能 & 特性**

合并、压缩、解析 css, js 文件。

合并：将多个 js 或 css 文件压缩打包成单一文件，減少 http request 的大小与数量。 
<br>
压缩：可以去除空格、注释等。 
<br>
解析：你可直接使用 SCSS 和 CoffeeScript, 它们会被解析成 css 和 js.

**主要通过 Sprockets 完成**

Assets Pipeline 的功能主要由重要的组件 Sprockets 完成。  
Sprockets 用来从你的 assets 路径打包压缩你所有的 assets 后包裝成一个文件，然后放到你目的地路径(public/assets).

通过它可以对 css, js 等静态资源进行编译、压缩。

命令行取消，不使用它：

```
rails new appname --skip-sprockets
```

这会使得原来的：

```ruby
require 'rails/all'
```

变成

```ruby
require "rails"
# Pick the frameworks you want:
require "active_model/railtie"
require "active_job/railtie"
require "active_record/railtie"
require "action_controller/railtie"
require "action_mailer/railtie"
require "action_view/railtie"
# require "sprockets/railtie" # <- 重点是这行
require "rails/test_unit/railtie"
```

sprockets 已经被取消掉。

并且，原来的：

```ruby
gem 'sass-rails'
gem 'uglifier'
gem 'coffee-rails'
```

变成：

```ruby
gem 'coffee-rails', '~> 4.1.0'
```

**默认的 3 个存放静态资源的地方**

默认 3 路径：
<br>
app/assets 放置我们自己所写的 js、css 或 images, 并且它们和业务关联比较紧密。实际上，这种形式用得最多。
<br>
lib/assets 放置我们抽取出来的 assets, 一般来说这样的代码比较通用。实际上，这种形式用得最少。
<br>
vendor/assets 是放一些我们从第三方引进的 assets，例如一些 jQuery 插件。

开发环境，你可以把你的 rails app 跑起來后，从 HTML 源代码里查看引入了那些样式或脚本，进而修改，很方便。

这 3 个目录，看似是分开的。但生产环境下，它们后会被编译、打包在一起，也就是说，它们其实是连通的。

** .erb 使用 Asset Helper 方法**

因为 Sprockets-rails 已经引入了以下两个模块

```ruby
include ActionView::Helpers::AssetUrlHelper
include ActionView::Helpers::AssetTagHelper
```

所以，在 *css.erb 文件里，你都可以使用以下方法：

```
stylesheet_link_tag
javascript_include_tag
```

```
image_tag
```

```
asset_path
asset_data_uri
```

同样的，使用 js.erb 后缀之后可以用 `asset_path` 等方法。

使用举例：

```ruby
audio_path("horse.wav")  # => /audios/horse.wav
audio_tag("sound")       # => <audio src="/audios/sound" />

font_path("font.ttf")    # => /fonts/font.ttf

image_path("edit.png")   # => "/images/edit.png"
image_tag("icon.png")    # => <img src="/images/icon.png" alt="Icon" />

video_path("hd.avi")     # => /videos/hd.avi
video_tag("trailer.ogg") # => <video src="/videos/trailer.ogg" />
```

**sass-rails 提供的几个 Asset Helper 方法**

由 sass-rails 提供几个以 `-url` 和 `-path` 结尾的 Asset Helpers 方法及结果：

```ruby
image-url("rails.png") # => url(/assets/rails.png)
image-path("rails.png") # => "/assets/rails.png".

asset-url("rails.png") # => url(/assets/rails.png)
asset-path("rails.png") # => "/assets/rails.png"

asset-data-url("rails.png") # => url(data:image/png;base64,iVBORw0K...)
```

注意，你可以同时使用 sass-rails 提供的 `-url` 及原生的 `url` 方法；使用 `-url` 时参数带引号，使用 `url` 时参数不带引号。

**Sprockets 提供 require 等指令**

```ruby
require # 引入某个文件。如果有重复引入，它会自动忽略，只引入一次。
require_directory # 引入某个目录下的文件，不会递归其子目录。
require_tree # 引入某个目录及递归其子目录下的所有文件，默认指的是当前目录。
require_self # 引入在自己文件里写的样式或脚本。

link

depend_on
depend_on_asset

stub
```

上述几个方法，由 Sprockets-rails 提供。

但上述几个方法，不要在 SASS/SCSS 文件里使用，而是使用 sass-rails 提供的 @import 方法。

**根据后缀名，决定编译顺序**

注意后缀名的顺序：

```ruby
app/assets/javascripts/projects.js.erb.coffee
```

从右到左一个个解析的。

**生产环境，需要注意的点**

1) Precompiling Assets

```ruby
RAILS_ENV=production bin/rake assets:precompile
```

```ruby
load 'deploy/assets'
```

另，上述会生成、使用 shared/assets 目录。

预编译的文件，默认是：

```ruby
[ Proc.new { |filename, path| path =~ /app\/assets/ && !%w(.js .css).include?(File.extname(filename)) },
/application.(css|js)$/ ]
```

新增：

```ruby
Rails.application.config.assets.precompile += ['admin.js', 'admin.css', 'swfObject.js']
```

另，只使用 .js 或 .css 后缀即可。不必减少，也不必增多。

2) Far-future Expires Header

配置 Web 服务器，更好的对待静态资源：

```ruby
location ~ ^/assets/ {
  expires 1y;
  add_header Cache-Control public;
 
  add_header ETag "";
  break;
}
```

**CDN 及异地静态资源**

把静态资源放到其它服务器：

```ruby
config.action_controller.asset_host = 'mycdnsubdomain.fictional-cdn.com'

config.action_controller.asset_host = ENV['CDN_HOST']
```

注意其影响：

```ruby
<%= asset_path('smile.png') %>

http://mycdnsubdomain.fictional-cdn.com/assets/smile.png

# 仅作用于指定的静态资源
<%= asset_path 'image.png', host: 'mycdnsubdomain.fictional-cdn.com' %>
```

另，如果生产环境有图片，而开发环境没有，而自己又不想导图片。可以尝试用这种方式，在开发环境也显示图片。

**两种引入方式**

1）独立引入

```ruby
config.assets.precompile += %w( site.css )
```

```ruby
stylesheet_link_tag "site"
```

2）一起引入

```ruby
# application.css

// = require_self
// = require 'site'
```

当然了，也可以直接把文件放到 public/assets/ 目录下，之后这些文件不受 Assets precompile 影响。

### sprockets-rails

**Railtie**

以 Railtie 的形式引入，继承于 Rails::Railtie （所以，Rails::Railtie 提供的方法，它是可以用的）

主要任务包括，但不限于：

- 设置 config.assets.x (这里的 x 表示众多的配置项)
- 其它众多看不见的功能

上述配置，包括但不限于：

```ruby
config.assets.precompile
config.assets.paths
config.assets.version
config.assets.prefix
config.assets.manifest
config.assets.digest
config.assets.debug
config.assets.compile
config.assets.configure
```

**打开了 Rails 下的 Engine 和 Application**

Engine 主要是加入路径：

```ruby
paths["vendor/assets"]
paths["lib/assets"]
paths["app"]
paths["app/assets"]
```

Application 主要提供以下几个 config 项：

```
:assets
:assets_manifest
:precompiled_assets
```

**如何引入**

它是 gem，所以：

```ruby
gem 'sprockets-rails', :require => 'sprockets/railtie'
```

**Rake task**

由 Task 文件完成。

```ruby
rake assets:precompile
rake assets:clean
rake assets:clobber
```

命令

```
RAILS_ENV=production bin/rake assets:precompile
```

将 app/assets 存放普通的前端资源复制到 public/assets 目录。

**Helper**

包括但不限于：

```ruby
include ActionView::Helpers::AssetUrlHelper
include ActionView::Helpers::AssetTagHelper
include Sprockets::Rails::Utils
      
VIEW_ACCESSORS = [:assets_environment, :assets_manifest,
                  :assets_precompile, :precompiled_assets,
                  :assets_prefix, :digest_assets, :debug_assets]
```

### 一些配置项

**rails scaffold 或 rails g controller 时不再生成默认的 js, css 文件**

```ruby
config.generators do |g|
  g.assets false
end
```

**显性配置 css, js 压缩器**

```ruby
config.assets.css_compressor = :yui
config.assets.js_compressor = :uglifier
```

```ruby
config.assets.css_compressor = :yui
```

```ruby
config.assets.css_compressor = :sass
```

JavaScript Compression

:closure, :uglifier 和 :yui

分别对应

closure-compiler, uglifier 和 yui-compressor gem.

```ruby
config.assets.js_compressor = :uglifier
```

**如何新增静态资源所在路径：**

如何新增：

```ruby
config.assets.paths << Rails.root.join("lib", "videoplayer", "flash")
```

**Runtime Error Checking**

```ruby
config.assets.raise_runtime_errors = false
```

没必要关掉吧。

**Turning Debugging Off**

```ruby
config.assets.debug = false
```

没必要关掉吧。

**Live Compilation**

生产环境，实时编译。

开发、测试等环境，由 coffee-script and sass 实时编译。

反正我是不推荐：

```ruby
config.assets.compile = true
```

生产环境 app/assets 下的文件已经预编译好，放到了 public/assets 下，直接使用即可。

**CDNs and the Cache-Control Header**

```ruby
config.static_cache_control = "public, max-age=31536000"
```

**Changing the assets Path**

```ruby
config.assets.prefix = "/some_other_path"
```

这也没必要吧。

**Assets Cache Store**

```ruby
config.assets.cache_store = :memory_store
```

```ruby
config.assets.cache_store = :memory_store, { size: 32.megabytes }
```

```ruby
config.assets.configure do |env|
  env.cache = ActiveSupport::Cache.lookup_store(:null_store)
end
```

一般情况下，也不会动这里哈~

**ssets Pipeline 怎么关掉?**

你可以在 confing/application.rb 中把他关掉：

```ruby
config.assets.enabled = false
```

### 其它

**查看所有 assets 目录**

当使用第三方 gem 引入 assets 资源的时候，使用它可以让我们看到加入了哪些目录。

```
Rails.application.config.assets
```

> Note：它只管加载目录，想引入目录下面资源文件的话，还得自己或 gem 添加。

**Deploy 的小技巧**

1）本地编译 - 有好也有坏。反正我是不推荐。

2）如果 assets 沒有更新，就不要跑 precompile.

3) 有更新就在本地 precompile，然后再上传。

**慎用 require_tree**

```ruby
# application.css
//= require_tree

# 相当于
//= require_tree .
```

意味着加载当前目录下的文件，并且递归加载其子目录下的文件。虽然省事了，这并不是好的实践。因为我们的文件、目录是会逐渐增多的。这就容易出现下列结果：

- 加载过多，导致性能慢
- 加载过多，导致混淆（原本没有必要加载的文件也加载了）

**css & scss & sass**

后缀名 .scss 意义 Sassy CSS
<br>
(还有一种后缀名为 .sass 的，区别是它严格缩进)

根据原有文件后缀名，选择不同的使用方式：

```ruby
# 1 application.css
*= require font-awesome
```

```ruby
# 2 application.css.scss
@import "font-awesome";
```

```ruby
# 3 application.css.sass
@import font-awesome
```

**检测 assets 是否挂了（存在）的命令**

图片"rails.png"存在

```ruby
Rails.application.assets.find_asset 'rails.png'
=> #<Sprockets::StaticAsset:0x3fed3aa004f8 
pathname="/Users/kelby/appname/app/assets/images/rails.png",
mtime=2015-04-13 20:10:42 +0800, digest="3526faae1dacebb591431f0054e8f33e">
```

图片"not-image.png"不存在

```ruby
Rails.application.assets.find_asset 'not-image.png'
 => nil
```

**没有 JavaScript 运行时**

```ruby
group :production do
  gem 'therubyracer'
end
```

但不推荐这么做，还是安装的好。

**X-Sendfile Headers**

Web 服务器支持的话，不妨一试：

```ruby
# config.action_dispatch.x_sendfile_header = "X-Sendfile" # for Apache
# config.action_dispatch.x_sendfile_header = 'X-Accel-Redirect' # for NGINX
```

当你使用 `send_file` 等方法给客户端发送文件时，如果你开启了此特性，并且硬盘里有恰好又有此文件。那么，你的 Web服务器会忽略应用的响应数据，直接从硬盘读取文件并返回，使得速度更快。

**不好的实践**

以 controller 为单位加载资源

```ruby
<%= javascript_include_tag params[:controller] %>
<%= stylesheet_link_tag params[:controller] %>
```

在我看来，上述方式并不好。

**CSS 里引入图片、字体**