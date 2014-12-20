## Timestamps

时间戳包括 `created_at/created_on` 和 `updated_at/updated_on`
  
不想生成时间戳相关属性:

```ruby
config.active_record.record_timestamps = false
```

时间戳默认使用 UTC，如果你想使用本地时区：

```ruby
config.active_record.default_timezone = :local
```

默认，ActiveRecord 可以自动识别并转换时区：

```ruby
config.active_record.time_zone_aware_attributes = true
```

如果你想取消此特性，可以配置为 `false`

如果你仍然想使用此特性，但同时又有需要忽略此特性，你可以手动设置，举例：
  
```ruby
class Topic < ActiveRecord::Base
  self.skip_time_zone_conversion_for_attributes = [:written_on]
end
```
