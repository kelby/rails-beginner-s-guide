## Mime Type register

**设置响应类型 Mime Type.**

先看看 `register` 方法

第 1，2 个参数容易理解<br>
第 3 个为标准类型<br>
第 4 个为扩展类型<br>

使用举例：

```ruby
Mime::Type.register "text/html", :html, %w( application/xhtml+xml ), %w( xhtml )
```

`register` 只是登记，本身是没有处理能力的，还需要使用 Action Controller 添加渲染器并处理：

```ruby
ActionController::Renderers.add ...
```

查看 Rails 支持什么类型的 MIME(Multipurpose Internet Mail Extensions，多用途互联网邮件扩展)：

```ruby
Mime::SET

=> [#<Mime::Type:0x007fdf7f01d9f0
  @string="text/html",
  @symbol=:html,
  @synonyms=["application/xhtml+xml"]>,
 #<Mime::Type:0x007fdf7f01d6f8
  @string="text/plain",
  @symbol=:text,
  @synonyms=[]>,
 #<Mime::Type:0x007fdf7f01d400
  @string="text/javascript",
  @symbol=:js,
  @synonyms=["application/javascript", "application/x-javascript"]>,
 #<Mime::Type:0x007fdf7f01d158 @string="text/css", @symbol=:css, @synonyms=[]>,
 ... ...
```

