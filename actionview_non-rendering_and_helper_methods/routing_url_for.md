## Routing Url For

`url_for`

它生成的内容是 url，区别于 link_to 等生成的是链接。

可选参数的类型可以是：Hash、String、nil、:back、Array 等，根据不同参数，可能会用到 ActionDispatch::HelperMethodBuilder 里的东西。

> Note: 有多个 url_for 方法，这个是 View 里用到的。
