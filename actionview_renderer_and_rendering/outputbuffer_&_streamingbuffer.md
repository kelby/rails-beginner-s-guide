## Output Buffer & Streaming Buffer

普通字符串使用 `html_safe` 后就变成了 Safe Buffer 的实例对象。

Output Buffer 继承于 Safe Buffer，它封装了一些方法，Rails 视图里使用的就是它。`html_safe?` 和 Safe Buffer 并不是一对一关系，所以会出现 Safe Buffer 实例对象的 html_safe? 为 false 的情况(这同样影响了子类 Output Buffer)；并且视图里字符串拼接(concat)对性能也会有影响。

基于种种原因，和 Output Buffer 相对的引入了 Streaming Buffer，但它以'流'的形式传递数据到客户端，并且它不继承于 Safe Buffer，所以绕开了一些问题。

参考

[YAGNI methods slow us down](http://tenderlovemaking.com/2014/06/04/yagni-methods-slow-us-down.html)
<br>
[ActionView Safe Buffer](http://thekaiway.com/2013/08/17/actionview-safe-buffer/)
