## ~~Collector~~

我们在 `respond_to` 里可以响应的格式 Mime.

Collector 是 Abstact Controller 实现的。Action Controller 有自己的扩展，Active Mailer 也有自己的扩展。

对应：

```ruby
# action 里面
respond_to do |format|
  format.html
  format.xml { render xml: @people }
end
```

调用 `format.html` 和 `format.xml` 等方法调用时会触发 `method_missing`，之后由 `generate_method_for_mime` 元编程生成。

另外，如果请求没有指定格式默认会按 format 里顺序执行，所以注意一下顺序。像一些搜索引擎就没有指定请求格式，所以一般的放 `format.html` 在前面。

目前 Rails 支持的 MIME(多用途互联网邮件扩展)，有：

>html
text
js
css
ics
csv
vcf
>
png
jpeg
gif
bmp
tiff
>
mpeg
>
xml
rss
atom
yaml
>
multipart_form
url_encoded_form
>
json
>
pdf
zip

可以在 action_dispatch/http/mime_types.rb 里查看。
