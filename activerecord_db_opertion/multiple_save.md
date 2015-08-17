## 多个 save 方法

在以下几个类或模块里都有 save 方法，那么它到底是如何工作，如何保存数据的呢。

```ruby
module ActiveRecord
  class Base
    # ... ...
    # 真正的保存 create_or_udpate
    include Persistence

    # ... ...
    # 相关校验 perform_validations
    include Validations

    # ... ...
    # 相关脏数据 Dirty
    include AttributeMethods

    # ... ...
    # 加上事务 rollback_active_record_state! 及 with_transaction_returning_status
    include Transactions

    # ... ...
  end
end
```

根据 Ruby 的代码执行规则：会"反着"模块的引入顺序，嵌套执行里面的 save 方法。

```
进入 Transactions 的 save
进入 AttributeMethods 的 save
进入 Validations 的 save
进入 Persistence 的 save

离开 Persistence 的 save
离开 Validations 的 save
离开 AttributeMethods 的 save
离开 Transactions 的 save
```

同理，当执行其它某个操作的时候，也会发生类似情形。

参考

[Life of save in ActiveRecord](http://blog.bigbinary.com/2013/01/15/live-of-save-in-activerecord.html)
