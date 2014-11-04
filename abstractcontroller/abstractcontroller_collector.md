## Collector

我们的响应格式。

```ruby
# in your actions
respond_to do |format|
  format.html
  format.xml { render xml: @people }
end
```

目前 Rails 支持的 MIME(多用途互联网邮件扩展)。

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

(可以在 action_dispatch/http/mime_types.rb 查看目前支持的格式)
