## 高级 - 11 个 Association 文件

**关联方法带来的高级的方法。**

**对外提供接口。**

概念：a.b 或 a.bs 组成一个 Association，把它们看成是一个整体。但前者为 owner，后者为 target.

实现对关联对象的操作。

#### 1) Association

```
attr_reader :owner, :target, :reflection
attr_accessor :inversed

delegate :options, :to => :reflection
```

```
aliased_table_name

reset

reload
loaded?
loaded!

stale_target?

target=

scope

association_scope

reset_scope

set_inverse_instance

klass

target_scope

load_target

interpolate

marshal_dump
marshal_load

initialize_attributes
```

#### 2) Belongs To Association

```
handle_dependency

replace
reset

updated?

decrement_counters
increment_counters
```

#### 3) Belongs To Polymorphic Association

```
klass
```

#### 4) Collection Association

```
reader
writer

ids_reader
ids_writer

reset
select

find

first
second
third
fourth
fifth
forty_two
last

build
create
create!

concat

transaction

delete_all
destroy_all

count

delete
destroy

size
length

empty?
any?
many?

distinct
uniq

replace

include?

load_target
add_to_target
replace_on_target

scope
null_scope?
```

#### 5) Foreign Association

```
foreign_key_present?
```

#### 6) Has Many Association

```
handle_dependency

insert_record

empty?
```

#### 7) Has Many Through Association

```
size
concat

concat_records
insert_record
```

#### 8) Has One Association

```
handle_dependency

replace
delete
```

#### 9) Has One Through Association

```
replace
```

#### 10) Singular Association

```
reader
writer

create
create!

build
```

#### 11) Through Association

```
delegate :source_reflection, :through_reflection, :to => :reflection
```
