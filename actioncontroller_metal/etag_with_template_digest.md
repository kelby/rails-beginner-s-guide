## Etag With Template Digest

**用 Digestor 给当前静态模板(对应 controller_name/action_name)加密，使之构成 Etag 的一部分。**

前面提到的 `fresh_when` 匹配的只是动态内容。静态内容怎么生成 cache_key，怎么过期？(比如开发环境下，我们会经常修改静态内容)

答案是：给静态内容做 md5 加密。每一次更改静态内容，对应的 Hash 值都会改变。在开发环境下，每次更新都会刷新页面；在生产环境下，重启服务器后会刷新。

默认该特性已启用，也就是说 Action View 静态内容已经加密(区别于片段缓存里对静态内容的加密)。

可以设置取消：

```ruby
config.action_controller.etag_with_template_digest = false
```

取消后，静态内容不经过加密，更改它们，Hash 值并不会更改。就会导致生产环境下，即使重启服务器得到的还是原来的内容。

本设置影响的是 ETag，缓存的是当前用户浏览的内容，他人或使用新浏览器访问页面，不受影响。

用到了 ConditionalGet::etag 和 Digestor::digest，注意它只用于条件判断，本身不会更改内容。
