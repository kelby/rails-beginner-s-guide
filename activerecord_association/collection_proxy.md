## Collection Proxy

通过某个对象，对其关联的对象进行操作。

它继承于 Relation，只是稍微做封装，所以接口上也有很多类似之处，**对外提供接口。**

有破坏行为：

```
<< & push & append
prepend

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

include?

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

proxy_association
```
