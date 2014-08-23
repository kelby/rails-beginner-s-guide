# ActionDispatch

路由相关是它的老本行。
对内：RouteSet
对外：除 RouteSet 外，routing 目录里的其它模块

外部请求进来的第一道和第二道闸门。
第一道：Middleware
第二道：Http

---

四部分

## Http

## Middleware

## RouteSet

- 物指 route_set.rb
- 本身就充满魔法
- 还是内外沟通的桥梁
- 内指 Journey
- 外指对外的接口及 routing 目录里的其它内容

## Routing

- 除 route_set.rb 外，routing 目录里的其它模块
- 对外提供接口
