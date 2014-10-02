# ActionDispatch RouteSet

## class Dispatcher

每一个路由规则转换着 `draw` 对应一个 Dispatcher 实例。

重要的转发方法 `dispatch`，将战场切换到 Controller#action

## class NamedRouteCollection

### 各个实例方法
### UrlHelper

## module MountedHelpers

## class Generator

各个实例方法

## 各个实例方法

### 路由已经定义，请求过来如何匹配？

之前用的是正则匹配，例如
 `/pictures/A12345` 可以匹配到 `get 'pictures/:id' => 'pictures#show', :constraints => { :id => /[A-Z]\d{5}/ }`
 
后来引入了 journey, 性能及其它各方面都有了很大的改善。
