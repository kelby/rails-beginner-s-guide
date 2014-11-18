## Data Streaming

```
send_data # 封装了方法 render
send_file # 封装了内置库 File
```

发送二进制数据或文件给浏览器，功能类似 render.
application/octet-stream（任意的二进制数据）
多用途互联网邮件扩展（MIME，Multipurpose Internet Mail Extensions）
1. type(决定 content_type，必须是 Mime::Type.register)
2. filename(如果没有 1，那么尝试根据它决定 content_type;更友好)
3. disposition(默认 attachment, 对应 Content-Disposition)
4. Content-Transfer-Encoding(写死的 binary)
5. :status (状态码，太常见了；默认是 200)

```
     disposition := "Content-Disposition" ":"
                    disposition-type
                    *(";" disposition-parm)

     disposition-type := "inline"
                       / "attachment"
                       / extension-token
                       ; values are not case-sensitive

     disposition-parm := filename-parm
                       / creation-date-parm
                       / modification-date-parm
                       / read-date-parm
                       / size-parm
                       / parameter

     filename-parm := "filename" "=" value

     creation-date-parm := "creation-date" "=" quoted-date-time

     modification-date-parm := "modification-date" "=" quoted-date-time

     read-date-parm := "read-date" "=" quoted-date-time

     size-parm := "size" "=" 1*DIGIT

     quoted-date-time := quoted-string
                      ; contents MUST be an RFC 822 `date-time'
                      ; numeric timezones (+HHMM or -HHMM) MUST be used
```

上面的消息头等就能看出它们其实差不多，可以从用途简单区分它们。

实际项目里，静态资源可以通过 Web服务器(Nginx/Apache)发送，应用只要提供 URL 即可。

它们发送方式类似。

但 send_data 可以发送非文件，也就是说即使是字符串也可发送。(比如动态生成的内容)

而 send_file 只能先有文件，才能发送。

当我们多次下载时，使用 send_file 比较好，应该它还可以充分利用 Web服务器，对性能友好。(比如'真正的'静态内容)

参考 [Difference between rails send_data and send_file, with example](http://stackoverflow.com/questions/5535981/difference-between-rails-send-data-and-send-file-with-example)
