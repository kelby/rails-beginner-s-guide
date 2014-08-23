## Rails.env.xxx?

举例：Rails.env.production?

仅作用于 Rails.env 对象<br/>
是 Rails.env == 'production' 的语法糖<br/>
动态生成的，所以 API 里查询不到<br/>
手法是：把方法最后的问号去掉，看是否和当前环境一样
