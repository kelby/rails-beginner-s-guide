## Mime Responds

### 实例方法

```
respond_to
```

`respond_to(*mimes, &block)` - 全部内容(包含类型和内容)

respond_to 可以指定 format, 如 html. 而在 format 里又可以指定 variant, 如：phone.

```ruby
format.html.phone # 行内风格

# 或

format.html{ |variant| variant.phone } # 代码块风格
```

### Collector

常用代码：

```
respond_to do |format|
  # format.class => ActionController::MimeResponds::Collector
  # ...
end
```

可见，`format` 是其实例对象。

```
all & any

custom

negotiate_format

response
```

此外，format.html 等对应着 Collector，而变种 format.html.phone 等对应着 Variant Collector.
