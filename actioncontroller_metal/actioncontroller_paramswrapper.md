## Params Wrapper

**对请求过来的 params 进行预处理。**通过 API 发送请求的话，使用它，特别方便。

**类方法：**

```
wrap_parameters
```

比如，实际发送的是：

```
{"name": "Konata"}
```

预处理过后，可以变成：

```
{"user": {"name": "Konata"}}
```

或变成：

```
{"name" => "Konata", "user" => {"name" => "Konata"}}
```

或变成其它样子，数据类型也可以改变。

开放 API 时，此特性用得最多，此时请求格式一般为 'application/json'

使用举例：

```ruby
# 默认
wrap_parameters false

# 原 params 复制一份，使用 :person 为 root 元素
wrap_parameters :person

# 原 params 复制一份，使用 :person 为 root 元素
wrap_parameters Person

# 原 params 复制一份，转化成 XML 格式，使用默认的 root 元素
wrap_parameters format: :xml

# 原 params 复制一份，但只要 :username 和 :title 部分，使用默认的 root 元素
wrap_parameters include: [:username, :title]

# 原 params 复制一份，但排除 :title 部分，使用默认的 root 元素
wrap_parameters :exclude => :title
```

`include_root_in_json` 一个功能上恰好和它有类似之处的方法(一个在 Active Model，另一个在 Action Controller)。

```ruby
Post.to_json

# 默认，没有 root 元素
{title: 'hello world'}

# 如果 include_root_in_json = true
{"post": {title: 'hello world'}}
```
