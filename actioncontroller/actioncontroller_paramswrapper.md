## Params Wrapper

```
process_action
```

## Class Methods

```
wrap_parameters
```

### Params Wrapper

我们用 POST 请求数据时，可以看到在 params 里对象封装在一个 root 元素里，例如：

```
"post" => {"title" => "hello world"}
```

我们获取里面的某个属性，只能这样

```ruby
params[:post][:title]

# 或

post = Post.new(params[:post])
post.title
```

在不引起误解的情况下，我们希望是能够通过 `params[:title]` 直接获取这个元素。当然这使用场景很有限。

```ruby
# enables the parameter wrapper for XML format
wrap_parameters format: :xml

# wraps parameters into params[:person] hash
wrap_parameters :person

# wraps parameters by determining the wrapper key from Person class
wrap_parameters Person

# wraps only :username and :title attributes from parameters.
wrap_parameters include: [:username, :title]

# disables parameters wrapping for this controller altogether.
wrap_parameters false
```

`include_root_in_json` 一个功能上恰好和它相反的配置方法(此外，一个在 ActiveModel，另一个在 ActionController)。

```ruby
Post.to_json

# 默认，没有 root 元素
{title: 'hello world'}

# 如果 include_root_in_json = true
{"post": {title: 'hello world'}}
```
