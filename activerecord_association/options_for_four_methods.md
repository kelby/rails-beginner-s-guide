## 关联方法的可选参数汇总

|      参数                        | has_one | has_many | belongs_to | habtm |
|----------------------------------|:-----:|:--------:|:--------:|:-----------:|
|:class_name                           |   √   |    √     |    √  |  √|
|:foreign_key                          |   √   |    √     |    √  |  √|
|:validate                             |   √   |    √     |    √  |  √|
|:autosave                             |   √   |    √     |    √  |  √|
|:primary_key                          |   √   |    √     |    √  |   |
|:dependent                            |   √   |    √     |    √  |   |
|:inverse_of                           |   √   |    √     |    √  |   |
|:as                                   |   √   |    √     |       |   |
|:through                              |   √   |    √     |       |   |
|:source                               |   √   |    √     |       |   |
|:source_type                          |   √   |    √     |       |   |
|:required                             |   √   |          |    √  |   |
|:counter_cache                        |       |    √     |    √  |   |
|:polymorphic                          |       |          |    √  |   |
|:touch                                |       |          |    √  |   |
|:foreign_type                         |   √   |    √     |    √  |   |
|:join_table                           |       |          |       |  √|
|:association_foreign_key              |       |          |       |  √|
|:readonly                             |       |          |       |  √|
