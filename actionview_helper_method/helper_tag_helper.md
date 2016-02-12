### Tag Helper

几乎所有生成 HTML 元素的 helper 方法，都封装它们而来。

```
content_tag

tag
```

用得比较多的是 `content_tag`(有结束标签) 和 `tag`(没有结束标签)。

`content_tag` 用于生成一个闭合的 HTML 标签。很多 helper 方法，都是封装于它。
<br>
`tag` 用于生成一个打开的 HTML 标签。很多 helper 方法，都是封装于它。

```
cdata_section

escape_once
```
