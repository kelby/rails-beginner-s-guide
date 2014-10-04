# ActiveRecord 约定、配置

## Naming Conventions

类映射表，属性映射列

命名约定：

|Model / Class |	Table / Schema|
|:----:|:---:|
|Post|	posts|
|LineItem|	line_items|
|Deer|	deers|
|Mouse|	mice|
|Person	|people|

## Schema Conventions

命名约定：

| 属性 | 解释 |
| -- | -- |
| Foreign keys | 外键。前者 belongs_to 后者，根据后者所对应的表名，为前者生成"后者_id"属性 |
| Primary keys | 主键。默认使用"id"属性，迁移时自动完成 |

除上述属性外，约定还有:

| 属性 | 解释 |
| -- | -- |
| created_at | 创建 record 的时间 |
| updated_at | 最后修改 record 的时间 |
| lock_version | 使用乐观锁特性的时候会用到 |
| type | 使用单表继承特性的时候会用到 |
| (association_name)_type | 使用多态特性的时候会用到 |
| (table_name)_count | 使用 counter_cache 特性的时候会用到 |

尽管这些属性都是可更改的，但强烈建议不要更改它们，使用默认的即可。  
另外，如果没有使用到上述特性，那么强烈建议不要使用上述属性。(实在需要的话，可以找同义词替代)

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
