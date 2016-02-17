## Remote Connections

它并不是什么新概念，只是针对使用了 `identified_by` 的连接，方便对其进行批量处理。

第一部分：

```
where
```

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user
    # ...
  end
end

# 查询、断开连接
ActionCable.server.remote_connections.where(current_user: User.find(1)).disconnect
```

第二部分：

```
disconnect
```

```
identifiers
```

它们只是调用，实际工作由 `server` 完成。