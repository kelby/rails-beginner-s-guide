## 多个 save 方法

在以下几个类或模块里都有 save 方法，那么它到底是如何工作，如何保存数据的呢。

```
module ActiveRecord
  class Base
    ......................
    include Persistence
    .......................
    include Validations
    ........................
    include AttributeMethods
    ........................
    include Transactions
    ........................
  end
end
```

参考

[Life of save in ActiveRecord](http://blog.bigbinary.com/2013/01/15/live-of-save-in-activerecord.html)
