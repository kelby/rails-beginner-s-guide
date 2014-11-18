## Persistence

很重要

```
becomes, becomes!

decrement, decrement!
increment, increment!

delete, destroy, destroy!, destroyed?

new_record?
persisted?

reload

save, save!

toggle, toggle!

touch

update & update_attributes
update! & update_attributes!

update_attribute

update_column
update_columns
```

数据库写操作，包括{update, destroy, create, save ...}

用原生 save 會有什麼問題呢？原生的 save 在資料儲存時，會經過一堆 validator 和 callbacks，即使你只是要簡單 update 一個欄位。

> Note: 这里大部分是对单个对象的操作。
