## Url For

对外提供 `url_for`，和 ActionView::RoutingUrlFor 有得一拼，原理一样，封装了 HelperMethodBuilder (极端情况下才调用到)

使用举例：

```ruby
url_for controller: 'tasks', action: 'testing', host: 'somehost.org', port: '8080'
# => 'http://somehost.org:8080/tasks/testing'

url_for controller: 'tasks', action: 'testing', host: 'somehost.org', anchor: 'ok', only_path: true
# => '/tasks/testing#ok'

url_for controller: 'tasks', action: 'testing', trailing_slash: true
# => 'http://somehost.org/tasks/testing/'

url_for controller: 'tasks', action: 'testing', host: 'somehost.org', number: '33'
# => 'http://somehost.org/tasks/testing?number=33'

url_for controller: 'tasks', action: 'testing', host: 'somehost.org', script_name: "/myapp"
# => 'http://somehost.org/myapp/tasks/testing'

url_for controller: 'tasks', action: 'testing', host: 'somehost.org', script_name: "/myapp", only_path: true
# => '/myapp/tasks/testing'
```

可选参数的类型可以是：nil、Hash、String、Symbol、Array、Class 等，根据不同参数，可能会用到 HelperMethodBuilder 里的东西。
