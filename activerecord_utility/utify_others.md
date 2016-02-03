## Readonly Attributes

提供方法：

```ruby
# 声明某些属性只读
attr_readonly
```

```ruby
# 获取所有只读的属性
readonly_attributes
```

`attr_readonly` 和其它 attr_x 类似，只不过这里设置的是某属性为只读。注意，这里不是校验，所以保存出错的话，不会放到 record 对象的 errors 里。
