## Collection Proxy

**对外提供接口。**

有破坏行为：

```
<< & push & append

to_a & to_ary

reset

replace

reload

create
create!

delete
delete_all

destroy
destroy_all

clear
```

仅是结果：

```
count

any?
many?
empty?

length
size

loaded?

==
```

查询：

```
new & build

concat

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

prepend

proxy_association

scope & spawn

scoping

select

take
```

其它：

```
arel

target

load_target
```
