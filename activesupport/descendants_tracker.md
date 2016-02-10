## ~~Descendants Tracker~~

查看某个类的子类，功能上和 Ruby 内置库 Object Space 类似，但性能上要比它好得多。

```ruby
class A
  extend ActiveSupport::DescendantsTracker
end

class B < A
end

class C < A
end

class D < B
end

# 输出 A 的所有子类
A.descendants
=> [B, D, C]

# 输出 A 的所有直接子类
A.direct_descendants
=> [B, C]
```

手法：重写 inherited 方法，当发生继承关系时，记录到 `@@direct_descendants` 里。
