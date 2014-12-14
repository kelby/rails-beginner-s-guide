## Renderers 增删渲染器

**类方法**

```
add
remove
```

举例一，使用 add 添加渲染器：

```ruby
ActionController::Renderers.add :csv do |obj, options|
  filename = options[:filename] || 'data'
  str = obj.respond_to?(:to_csv) ? obj.to_csv : obj.to_s
  send_data str, type: Mime::CSV,
    disposition: "attachment; filename=#{filename}.csv"
end
```

举例一，使用刚才添加的渲染器：

```ruby
def show
  @csvable = Csvable.find(params[:id])

  respond_to do |format|
    format.html
    format.csv { render csv: @csvable, filename: @csvable.name }
  end
end
```

举例二，移除渲染器：

```ruby
ActionController::Renderers.remove(:csv)
```

**实例方法**

```
render_to_body
```

**其它类方法**

```
use_renderer & use_renderers
```

默认 ActionController 带着的渲染器:

```ruby
ActionController::Renderers::RENDERERS
=> #<Set: {:json, :js, :xml}>
```
