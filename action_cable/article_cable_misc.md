## Remote Connections

第一部分：

```
where
```

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user
    ....
  end
end

ActionCable.server.remote_connections.where(current_user: User.find(1)).disconnect
```

第二部分：

```
disconnect()
```

```
identifiers()
```

它们只是调用，实际工作由 `server` 完成。