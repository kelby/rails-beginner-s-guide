## ~~Collector~~

我们的响应格式 Mime.

Collector 是 Abstact Controller 实现的。Action Controller 有自己的扩展，Active Mailer 也有自己的扩展。

对应：

```ruby
# action 里面
respond_to do |format|
  format.html
  format.xml { render xml: @people }
end
```

这里的 html 和 xml.
<br>
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
