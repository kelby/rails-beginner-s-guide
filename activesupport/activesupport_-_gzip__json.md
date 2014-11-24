## Gzip 与 JSON

封装了标准库 [zlib](http://ruby-doc.org/stdlib-2.1.0/libdoc/zlib/rdoc/index.html)，提供 gzip 压缩/解压缩字符串功能。

```ruby
# 压缩
gzip = ActiveSupport::Gzip.compress('compress me!')
# => "\x1F\x8B\b\x00o\x8D\xCDO\x00\x03K\xCE\xCF-(J-.V\xC8MU\x04\x00R>n\x83\f\x00\x00\x00"

# 解压缩
ActiveSupport::Gzip.decompress(gzip)
# => "compress me!"
```

有一个和它类似 JSON

`encode(value, options = nil)`
将对象转换成 JSON 格式

```ruby
# 加密
# 是模块方法
ActiveSupport::JSON.encode({ team: 'rails', players: '36' })
# => "{\"team\":\"rails\",\"players\":\"36\"}"
```

`decode(json, options = {})`
将 JSON 字符串转换成 Hash

```ruby
# 解密
# 是类方法
ActiveSupport::JSON.decode("{\"team\":\"rails\",\"players\":\"36\"}")
=> {"team" => "rails", "players" => "36"}
```
