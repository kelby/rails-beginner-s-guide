## Conversion

数据库提供的 record 并不是所有数据对我们都有用，有时候我们需要转换。常见的转换方法有：

```ruby
to_key
to_model
to_param

to_partial_path
```

举例 `to_param`
使用到它的一些方法：

```ruby
url_for

button_to
```

此时，会用 id 做为 url 的一部分，这对用户体验和 SEO 都不太友好，通常我们可以在 model 里覆盖此方法。

```
def to_param
  "#{id}-#{title.parameterize}"
end
```
