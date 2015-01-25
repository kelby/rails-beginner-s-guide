## Xml Mini

Active Support 允许你**使用不同的 XML 解析器**。

默认用的是标准的 REXML，不过你可以使用性能更快的 LibXML 或 Nokogiri. (如果你那么在意性能的话)

类方法(对外接口)：

```
backend
backend=

rename_key

to_tag

with_backend
```

此外，还有类方法：

```
delegate :parse, :to => :backend
```

`parse` 是主要对外接口。前台保持它不变即可，后台可随意更改解析器。

使用示例：

```ruby
gem 'libxml-ruby', '=0.9.7'
XmlMini.backend = 'LibXML'
```
