## Renderers 增删渲染器

针对 respond_to 里面不同的 format 会有不同的渲染器对它们进行处理。

**实例方法：**

```
render_to_body
```

`render_to_body` 在其它地方有同名方法，执行渲染时就会触发它，这里就隐藏着魔法。

还有实例方法 _render_to_body_with_renderer，在实现过程中很重要。

**类方法：**

```
add

remove
```

1) 使用 add 添加渲染器：

```ruby
ActionController::Renderers.add :csv do |obj, options|
  filename = options[:filename] || 'data'
  str = obj.respond_to?(:to_csv) ? obj.to_csv : obj.to_s

  send_data str, type: Mime::CSV,
                 disposition: "attachment; filename=#{filename}.csv"
end
```

2) 之后要注册：

```ruby
Mime::Type.register "application/csv", :csv
```

(在后面的 Mime Type register 章节还会提及)

3) 使用刚才添加的渲染器：

```ruby
def show
  @csvable = Csvable.find(params[:id])

  respond_to do |format|
    format.html
    # 对应这里的 format.csv
    format.csv { render csv: @csvable, filename: @csvable.name }
  end
end
```

移除渲染器：

```ruby
ActionController::Renderers.remove(:csv)
```

**其它类方法：**

```
use_renderer & use_renderers
```

默认 Action Controller 带着的渲染器:

```ruby
ActionController::Renderers::RENDERERS
=> #<Set: {:json, :js, :xml}>
```
