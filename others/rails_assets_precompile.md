## Rails assets precompile

#### Assets Pipeline 主要功能

合并、压缩、解析 css, js 文件。

合并：将多个 js 或 css 文件压缩打包成单一文件，減少 http request 的大小与数量。
<br>
压缩：可以去除空格、注释等。
<br>
解析：你可直接使用 SCSS 和 CoffeeScript, 它们会被解析成 css 和 js.

#### Assets Pipeline 相关 gem

Assets Pipeline 的功能主要由重要的组件提供：Sprockets
<br>
Sprockets 用来从你的 assets 路径打包压缩你所有的 assets 后包裝成一个文件，然后放到你目的地路径(public/assets).

#### Assets Pipeline 怎么关掉?

你可以在 `confing/application.rb` 中把他关掉：

```ruby
config.assets.enabled = false
```

#### Assets 的结构

首先必须了解 Assets 的结构，在 Rails 的目录结构中有三个地方：

- app/assets(通常放置我们自己所写的 js、css 或 images, 并且它们和业务关联比较紧密)
- lib/assets(通常放置我们抽取出来的 assets, 通常它们和业务关联不是很紧密)
- vendor/assets(通常是放一些我们从第三方引进的 assets，例如一些 jQuery 插件)

这 3 个目录，看似是分开，但它们是连通的(因为最后都要打包在一起)。

你可以把你的 rails app 跑起來后在 http://localhost:3000/assets/application.js 中看到你所有的 js 都在这个文件里，css 同理亦然。

其它路径，可以通过 `config.assets.paths` 添加，例如：

```ruby
Rails.application.config.assets.paths << Emoji.images_path
```

你可以在控制台里输入 

```ruby
Rails.application.config.assets.paths
```

来查看所有的 assets 路径。

#### Assets 的载入

主要有两种方式载入：
<br>
一，打包进 application.js，这种方式直接使用类似 require 命令引用即可；
<br>
二，自己做为一个单独文件，此方式需要在配置文件里加入 precompile，然后使用类似 stylesheet_link_tag 命令引入进 View 里。 

除了 `require` 外，你还有 `require_tree` 及 `require_directory` ，及其它的方法，你都可以使用绝对或是相对路径来指定文件位置(后缀名可以不设置)：

- require [路径] 引入某个文件。如果有重复引入，它会自动忽略，只引入一次。
- include [路径] 引入某个文件。如果有重复引入，它不会自动忽略。
- require_directory [路径] 引入当前路径下不包含子目录的文件。
- require_tree [路径] 引入当前路径下所有的文件。
- require_self [路径] 引入自己文件里写的样式或脚本。

你可以看 Sprockets 的 Readme 來了解更多使用方法。

#### Helper

Assets 提供了很多路径相关的 helper 來让你指向你的 assets:

```ruby
audio_path("horse.wav")  # => /audios/horse.wav
audio_tag("sound")       # => <audio src="/audios/sound" />
font_path("font.ttf")    # => /fonts/font.ttf
image_path("edit.png")   # => "/images/edit.png"
image_tag("icon.png")    # => <img src="/images/icon.png" alt="Icon" />
video_path("hd.avi")     # => /videos/hd.avi
video_tag("trailer.ogg") # => <video src="/videos/trailer.ogg" />
```

Sass 还提供了像是 `-url` 和 `-path` 這样的 helper 來帮助你，因此你也可以这样使用：

```ruby
image-url("rails.png")         # => url(/assets/rails.png)
image-path("rails.png")        # => "/assets/rails.png".
asset-url("rails.png", image)  # => url(/assets/rails.png)
asset-path("rails.png", image) # => "/assets/rails.png"
Production
```

1) 引入图片：

```
asset-path("rails.png") becomes "/assets/rails.png"
asset-url("rails.png") becomes url(/assets/rails.png)
```

```
image-path("rails.png") becomes "/assets/rails.png"
image-url("rails.png") becomes url(/assets/rails.png)
```

```
asset-data-url("rails.png") becomes url(data:image/png;base64,iVBORw0K...)
```

1，在 html.erb 里引入
2，在 css 里引入

#### 引用方式

不推荐使用 url 的方式，但一定要用的话，注意方式：

绝对路径：

```
url(/assets/iphonegala/search-btn.png)
```

相对路径：

```
url(images/ui-bg_flat_75_ffffff_40x100.png)
```

另，有 public/images 目录，引用：

```
url(/images/up-arrow.png)
```

再或者：

```
image-url('cover/cover-bg-8.jpg')
```

检测 assets 是否挂了（存在）的命令：

```ruby
Rails.application.assets.find_asset 'lolshirts/theme/bg-header.png'
 => #> Sprockets::StaticAsset:0x80c388ec pathname="/Users/joevandyk/projects/tanga/sites/lolshirts/app/assets/images/lolshirts/theme/bg-header.png", mtime=2011-10-07 12:34:48 -0700, digest="a63cc84aca38e2172ae25de3d837c71a">

Rails.application.assets.find_asset 'notthere.png'
 => nil
 ```





2) 引入 JS & 引入 CSS:

无论哪的文件，都只有上面两种方法。你要做一些改变，而且很小的。

1.1，在 html.erb 里引入

```ruby
# application.rb
config.assets.precompile += %w(team/manage.css team/manage.js)
```

```
# *.html.erb
javascript_include_tag
stylesheet_link_tag
```

1.2 在 html.erb 里引入

继承于 application, 不用做什么

```
//= require redactor-rails-config/config
//= require twitter/bootstrap
```

2.1，在 css 里引入

```ruby
# application.rb
config.assets.precompile += %w(team/manage.css team/manage.js)
```

```
# *.html.erb
javascript_include_tag
stylesheet_link_tag
```

2.2，在 css 里引入

继承于 application, 不用做什么

```ruby
*= require jisu_voting
```

(此方法由 sprockets 提供)

參考：

[RailsGuides Asset Pipeline](http://guides.rubyonrails.org/asset_pipeline.html)
<br>
[一次搞懂 Assets Pipeline](http://gogojimmy.net/2012/07/03/understand-assets-pipline/)
