# ActiveSupport - Gzip 与 JSON
A convenient wrapper for the zlib standard library that allows compression/decompression of strings with gzip.

```ruby
# 加密
gzip = ActiveSupport::Gzip.compress('compress me!')
# => "\x1F\x8B\b\x00o\x8D\xCDO\x00\x03K\xCE\xCF-(J-.V\xC8MU\x04\x00R>n\x83\f\x00\x00\x00"

# 解密
ActiveSupport::Gzip.decompress(gzip)
# => "compress me!"
```

只是对标准库 gzip 的简单封装，对外提供两个方法。

---------

有一个和它类似 JSON

`encode(value, options = nil)`
Dumps objects in JSON (JavaScript Object Notation). See www.json.org for more info.

```ruby
# 加密
# 是模块方法
ActiveSupport::JSON.encode({ team: 'rails', players: '36' })
# => "{\"team\":\"rails\",\"players\":\"36\"}"
```

`decode(json, options = {})`
Parses a JSON string (JavaScript Object Notation) into a hash. See www.json.org for more info.

```ruby
# 解密
# 是类方法
ActiveSupport::JSON.decode("{\"team\":\"rails\",\"players\":\"36\"}")
=> {"team" => "rails", "players" => "36"}
```

什么原因，它们一个是模块方法，一个是类方法
