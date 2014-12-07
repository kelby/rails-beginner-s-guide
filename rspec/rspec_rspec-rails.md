## rspec-rails

**View Rendering**

```
render_views
```

**View Assigns**

```
assign
```

定义一个实例变量并赋值。对应着在 Controlller 里定义实例变量并赋值，然后传递给 View 使用。

使用举例：

```ruby
assign(:widget, Widget.new)
```

**Matchers**

```
be_a_new
be_new_record
be_valid
be_routable

get
post
put
patch
delete
options
head

have_http_status
have_rendered & render_template

redirect_to
route_to
```

**Examples**

```
controller
routes

render
view
stub_template
params
template
response
```

链接 [rspec-rails](http://www.rubydoc.info/github/rspec/rspec-rails/)
