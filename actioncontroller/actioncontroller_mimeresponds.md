# MimeResponds

```
respond_to
```

`respond_to(*mimes, &block)` - 全部内容(包含类型和内容)

respond_to 可以指定 format, 如 html. 而在 format 里又可以指定 variant, 如：phone.

```ruby
format.html.phone - variant inline syntax

# 或

format.html{ |variant| variant.phone } - variant block syntax
```

html 等响应格式由 Collector 处理，而变种由 VariantCollector 来处理。

## ~~Collector~~

```
all, any
custom
negotiate_format, new
response
```
