## Xml Mini

`to_xml` 底层处理之一，以及允许你**切换使用不同的库进行解析 HTML/XML**.

默认用的是标准库 REXML，不过你可以使用性能更快的 LibXML 或 Nokogiri. (性能上快得多，不过有极少数不规范的网站可能会解析失败)

```ruby
ActiveSupport::XmlMini.backend
# => ActiveSupport::XmlMini_REXML
```

类方法(对外接口)：

```
backend
backend=

rename_key

to_tag

with_backend
```

此外，还有类方法：

```ruby
delegate :parse, :to => :backend
```

`parse` 是主要对外接口。前台保持它不变即可，后台可随意更改解析器。

使用示例：

```ruby
gem 'libxml-ruby', '=0.9.7'
XmlMini.backend = 'LibXML'
```

另，Rails 里的数组和哈希的 `to_xml` 也用到了它进行转换处理。

