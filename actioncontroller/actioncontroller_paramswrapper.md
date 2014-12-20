## ~~Params Wrapper 额外的 params[:x]~~

**类方法**

```
wrap_parameters
```

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

在不引起误解的情况下，我们希望是能够通过 `params[:title]` 直接获取这个元素。虽然这样的使用场景有限。

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

注意：**开发 API 等，我们希望传递的是 JSON 格式的数据。但处理时，我们又希望使用 params Hash，那 Rails 能不能自动帮我们转换呢？**

只有请求格式为 'application/json'，上述描述才起作用！

---

`include_root_in_json`<br> 一个功能上恰好和它相反的配置方法(此外，一个在 Active Model，另一个在 Action Controller)。

```ruby
Post.to_json

# 默认，没有 root 元素
{title: 'hello world'}

# 如果 include_root_in_json = true
{"post": {title: 'hello world'}}
```
