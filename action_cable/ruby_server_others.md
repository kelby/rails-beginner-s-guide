## Server 补充

### Connections

手动添加、移除链接，主要为了方便开发、调试，建议请不要做它用。

```
connections

add_connection
remove_connection

setup_heartbeat_timer

open_connections_statistics
```

### Configuration

实例对象：`ActionCable.server.config`

```ruby
attr_accessor :logger, :log_tags
attr_accessor :connection_class, :worker_pool_size
attr_accessor :redis, :channels_path
attr_accessor :disable_request_forgery_protection, :allowed_request_origins
attr_accessor :url
```

```ruby
channel_paths

channel_class_names

# 默认使用的适配器是 redis
pubsub_adapter
```

### Worker

```ruby
Server.send_async
```

### Active Record Connection Management

及时清理 Active Record 的链接，防止堆积成山。

