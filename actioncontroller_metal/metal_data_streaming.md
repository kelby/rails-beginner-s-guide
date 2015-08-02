## Data Streaming

```
send_data
send_file
```

`send_data` 类似 render :text
<br>
`send_file` 类似 response_body = file

它们发送方式类似，并且实现上也有相同的 `send_file_headers!`

但 `send_data` 可以发送的是数据，可以是非文件，也就是说即使是字符串也可发送。(比如动态生成的内容)

而 `send_file` 只能先有文件，才能发送。

相关可选参数：:filename、:type、:disposition、:status 或 :url_based_filename 可以查看 API 文档了解。

一般的，动态生成或一次性的内容，使用 send_data 比较好；纯文件或内容可供多次下载的，使用 send_file 比较好。

另，实际项目里，静态资源还可以通过 Web 服务器(Nginx/Apache)发送，应用只要提供 URL 即可。此时，仍然可以和原来一样调用 send_file 方法，但真正返回数据的时候，Web服务器会自动忽略掉应用服务器的 response.

```ruby
# config/environments/production.rb

config.action_dispatch.x_sendfile_header = "X-Sendfile" # for Apache
config.action_dispatch.x_sendfile_header = 'X-Accel-Redirect' # for NGINX
```
