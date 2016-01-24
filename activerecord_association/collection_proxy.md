## Collection Proxy

**对外提供接口。**

有破坏行为：

```
<< & push & append

to_a & to_ary

reset

replace

reload
```

仅是结果：

```
count

any?
many?
empty?

length
size

==
```

查询：

```
arel

new & build

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



prepend

proxy_association

scope & spawn

scoping

select

take
```

其它：

```
target
```
