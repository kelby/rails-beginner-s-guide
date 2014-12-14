## Text Helper

```
truncate      截取某个长度的文本

concat        求值

cycle         循环
current_cycle 循环内当前值
reset_cycle   中断循环，从头开始

excerpt       引用、摘录(只显示某个词及其附近的语句，其余用 ... 代替)
highlight     高亮(默认是加 mark )
pluralize     复数形式

safe_concat   在 ActiveSupport 里也提供了 safe_concat 方法，如果可用则用；否则用 concat
simple_format 文本简单格式化
word_wrap     限制长度
```
