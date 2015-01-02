## No Touching

常用方法：

```
no_touching
```

使用举例：

```ruby
ActiveRecord::Base.no_touching do
  Project.first.touch # 不会执行 touch
  Message.first.touch # 不会执行 touch
end

Project.no_touching do
  Project.first.touch # 不会执行 touch
  Message.first.touch # 会对 message 执行 touch; 但它 touch: true 的关联对象不会被 touch
end
```

除上述外，还有方法：

```
no_touching?

touch
# 优先级大于 Persistence 里的 touch 同名方法；
# 如果 no_touching? => true 则不调用 Persistence 里的 touch 方法。
```
