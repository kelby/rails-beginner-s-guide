# ActionDispatch RouteSet

## 各个实例方法

## Dispatcher

## Named Route Collection

## Generator

## ~~MountedHelpers~~

### 路由已经定义，请求过来如何匹配？

之前用的是正则匹配，例如
 `/pictures/A12345` 可以匹配到 `get 'pictures/:id' => 'pictures#show', :constraints => { :id => /[A-Z]\d{5}/ }`
 
后来引入了 journey, 性能及其它各方面都有了很大的改善。
