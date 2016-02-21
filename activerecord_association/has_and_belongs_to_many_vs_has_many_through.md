#### has_and_belongs_to_many vs has_many through

has_and_belongs_to_many 意味着中间表没有 model. 因此，不能通过 id 进行查询，也没有 validate、callback，更加不能直接读取&设置其值。唯一的方便之处是：通过 `<<` 即可创建中间表数据。

当你需要 model、id、validate、callback 或对中间表数据进行操作时，建议改为使用 has_many :throght，这时可以通过 model 管理中间表数据，不必也不能再 `<<`

| 关联      | has_and_belongs_to_many | has_many :through |
|----                |----                       |----               |
| AKA | habtm      | through association|
| Structure        | Join Table | Join Model|
| Primary Key | no | yes|
| Rich Association | no | yes|
| Proxy Collection | yes| no|
| Distinct Selection | yes | yes|
| Self-Referential | yes | yes|
| Eager Loading | yes| yes|
| Polymorphism |  no | yes|
| N-way Joins | no | yes |
