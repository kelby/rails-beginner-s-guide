## ~~endpoint 和 inspector 文件~~

endpoint 文件下的内容：

| 类 | 解释 |
| --- | --- |
| Endpoint | Mapper::Constraints、 Redirect 和 RouteSet::Dispatcher 的抽象类。<br> 定义了 dispatcher?、redirect?、matches? 和 app 方法。|

inspector 文件下的内容：

类 | 解释 
-- | -- 
Route Wrapper        | 被 Routes Inspector 调用。
Routes Inspector     | 被 middleware Debug Exceptions 和 rake routes 调用。
Html Table Formatter | 格式化 HTML 里的路由信息，给人阅读的。
Console Formatter   | 格式化控制台里的路由信息，给人阅读的。
