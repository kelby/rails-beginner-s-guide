## Mime Negotiation

在 new 页面，对 http://blog.test.example.com:3000/users 发起 POST 请求。

检测结果：

```ruby
request.accepts
=> [#<Mime::Type:0x007f8a1a498cd8 @string="text/html", @symbol=:html, \n
                                  @synonyms=["application/xhtml+xml"]>,
 #<Mime::Type:0x007f8a20f2f498 @string="image/webp", @symbol=nil, @synonyms=[]>,
 #<Mime::Type:0x007f8a1a48a908 @string="application/xml", @symbol=:xml, \n
                               @synonyms=["text/xml", "application/x-xml"]>,
 #<Mime::Type:0x007f8a20f2f3f8 @string="*/*", @symbol=nil, @synonyms=[]>]

request.content_mime_type
=> #<Mime::Type:0x007f8a1a47be58 @string="application/x-www-form-urlencoded", \n
                                 @symbol=:url_encoded_form, @synonyms=[]>

request.content_type
=> "application/x-www-form-urlencoded"

# = formats.first
request.format
=> #<Mime::Type:0x007f8a1a498cd8 @string="text/html", @symbol=:html, \n
                                 @synonyms=["application/xhtml+xml"]>

request.formats
=> [#<Mime::Type:0x007f8a1a498cd8 @string="text/html", @symbol=:html, \n
                                  @synonyms=["application/xhtml+xml"]>]
```

除上述方法外，还有：

```
negotiate_mime

format=
formats=
variant=
```

给“变种”增加了一些新的方法：

```ruby
request.variant = :phone
request.variant.phone?  # true
request.variant.tablet? # false

request.variant = [:phone, :tablet]
request.variant.phone?                  # true
request.variant.desktop?                # false
request.variant.any?(:phone, :desktop)  # true
request.variant.any?(:desktop, :watch)  # false
```
