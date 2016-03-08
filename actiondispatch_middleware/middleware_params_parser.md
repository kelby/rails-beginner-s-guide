### Params Parser

解析，并格式化请求过来的参数。

包括但不限于，当请求的数据格式是 JSON 类型时，进行转换：

```ruby
data = ActiveSupport::JSON.decode(request.raw_post)
data = {:_json => data} unless data.is_a?(Hash)

Request::Utils.deep_munge(data).with_indifferent_access
```

注意：不是所有参数，特指 `request.request_parameters`，也就是 POST 请求发过来的参数。
