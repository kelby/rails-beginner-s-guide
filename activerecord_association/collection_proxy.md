## Collection Proxy

**对外提供接口。**

有破坏行为：

```
<< & push & append

to_a & to_ary
```

仅是结果：

```
==

any?

count

empty?

length
size
```

查询：

```
arel

build

clear

concat

create
create!

delete
delete_all

destroy
destroy_all

distinct & uniq

find

first
last

second
third
fourth
fifth
forty_two

include?

load_target

loaded?

many?

new

prepend

proxy_association

reload

replace

reset

scope & spawn

scoping

select
```

其它：

```
target
```
