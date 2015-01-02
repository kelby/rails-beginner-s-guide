## Url For

对外提供 `url_for`，和 ActionView::RoutingUrlFor 有得一拼，原理一样，封装了 Helper Method Builder. (极端情况下才调用到)

使用举例：

```ruby
url_for controller: 'tasks', action: 'testing', host: 'example.org', port: '8080'
# => 'http://example.org:8080/tasks/testing'

url_for controller: 'tasks', action: 'testing', host: 'example.org',
        anchor: 'ok', only_path: true
# => '/tasks/testing#ok'

url_for controller: 'tasks', action: 'testing', trailing_slash: true
# => 'http://example.org/tasks/testing/'

url_for controller: 'tasks', action: 'testing', host: 'example.org', number: '33'
# => 'http://example.org/tasks/testing?number=33'

url_for controller: 'tasks', action: 'testing', host: 'example.org',
        script_name: "/myapp"
# => 'http://example.org/myapp/tasks/testing'

url_for controller: 'tasks', action: 'testing', host: 'example.org',
        script_name: "/myapp", only_path: true
# => '/myapp/tasks/testing'
```

可选参数的类型可以是：nil、Hash、String、Symbol、Array、Class 等，根据不同参数，可能会用到 Helper Method Builder 里的东西。
