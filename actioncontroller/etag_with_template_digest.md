## Etag With Template Digest

**页面相关的客户端缓存。**

用 Digestor 给当前静态模板(对应 controller_name/action_name)加密，使之构成 Etag 的一部分。

默认是 true，也就是说 Action View 静态内容已经加密(区别于片段缓存里对静态内容的加密)。
<br>
可以设置取消(尽管我想不出什么场景需要取消)：

```ruby
config.action_controller.etag_with_template_digest = false
```

fresh_when 匹配的只是动态内容。静态内容怎么办？(比如开发环境下，我们会经常修改静态内容)

静态内容经过加密，每一次更改，Hash 值都会改变，所以开发环境下每次更新会刷新，生产环境下重启服务器后会刷新。
取消后，静态内容不经过加密，更改它们，Hash 值并不会更改，就会导致(生产环境下)即使重启服务器得到的还是原来的内容。

本设置仅针对 ETag，若在其它浏览器访问网站，得到的仍然是正确内容。

用到了 ConditionalGet::etag 和 Digestor::digest，注意它只用于条件判断，本身不会更改内容。
