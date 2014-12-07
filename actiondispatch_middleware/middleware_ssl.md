**SSL**

如果网站采用了 HSTS(HTTP Strict Transport Security) 协议，该 middleware 保证通过浏览器访问时，始终连接到该网站的 HTTPS 加密版本(也就是，访问被强制 https 了)，不需要用户手动在 URL 地址栏中输入加密地址。

实际项目中，如果用不到 HSTS(HTTP 严格传输安全协议)，可以考虑将此 middleware 移除。
