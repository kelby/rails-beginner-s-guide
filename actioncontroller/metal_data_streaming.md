## Data Streaming

```
send_data
send_file
```

`send_data` 类似 render :text
<br>
`send_file` 类似 response_body = file

它们发送方式类似，并且实现上也有相同的 send_file_headers!

但 `send_data` 可以发送的是数据，可以是非文件，也就是说即使是字符串也可发送。(比如动态生成的内容)

而 `send_file` 只能先有文件，才能发送。

相关可选参数：:filename、:type、:disposition、:status 或 :url_based_filename 可以查看 API 文档了解。

一般的，动态生成或一次性的内容，使用 send_data 比较好；纯文件或内容可供多次下载的，使用 send_file 比较好。

另，实际项目里，静态资源还可以通过 Web 服务器(Nginx/Apache)发送，应用只要提供 URL 即可。

参考

[Difference between rails send_data and send_file, with example](http://stackoverflow.com/questions/5535981/difference-between-rails-send-data-and-send-file-with-example)
