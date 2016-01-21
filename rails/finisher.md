### Finisher 收尾

include by Application.

添加 generator templates  
Ensure autoload once paths as subset  
添加 builtin route  
构建 middleware stack  
**定义 main_app helper**(使用 Engine 时，一个比较重要的概念)  
添加 to prepare blocks  
运行 prepare callbacks  
Eager load!  
~~Finisher hook~~  
设置 routes reloader hook  
设置 clear dependencies hook

另，
<br>
`main_app` 可以让你确保使用的是主应用所在的环境，当你使用了 mountable Engine 时，这点可能很重要。
