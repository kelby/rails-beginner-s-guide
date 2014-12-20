## Schema Conventions

命名约定：

| 属性 | 解释 |
| -- | -- |
| Foreign keys | 外键。前者 belongs_to 后者，根据后者所对应的表名，为前者生成"后者_id"属性。 |
| Primary keys | 主键。默认使用"id"属性，迁移时自动完成。 |

除上述属性外，约定还有:

| 属性 | 解释 |
| -- | -- |
| created_at | 创建 record 的时间戳 |
| updated_at | 最后修改 record 的时间戳 |
| created_on | 创建 record 的时间 |
| updated_on | 最后修改 record 的时间 |
| lock_version | 使用乐观锁特性的时候会用到 |
| type | 使用单表继承特性的时候会用到 |
| (association_name)_type | 使用多态特性的时候会用到 |
| (table_name)_count | 使用 counter_cache 特性的时候会用到 |

尽管这些属性都是可更改的，但强烈建议不要更改它们，使用默认的即可。  
另外，如果没有使用到上述特性，那么强烈建议不要使用上述属性。(实在需要的话，可以找同义词替代)
