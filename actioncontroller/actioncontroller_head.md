# ActionController Head

## 返回一个内容为空的 response

有时候(作为 Web service 时)，响应内容可能只需要一个状态码，其它内容都不需要。

你可以使用 `render` 方法，这么做：

```ruby
headers['Location'] = person_url(@person)
render :nothing => true, :status => "201 Created"
```

但是，如果类似代码有很多地方的话，也会让人头疼。我们可以
直接用 `head` 方法:

```ruby
head :created, :location => person_url(@person)
```

> Note: 结果和使用 render 方法时 render nothing: true 类似。
