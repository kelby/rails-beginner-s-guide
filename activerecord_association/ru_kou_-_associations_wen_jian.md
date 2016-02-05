## Associations 文件 - 入口

Association 提供我们 4 个常用方法：

```
has_many
has_one
belongs_to
has_and_belongs_to_many
```

步骤：


```
-> 对应的 Builder，父 Builder, 顶级 Builder …（交叉进行的）
```

```
-> 对应的 Reflection，父 Reflection, 再父 Reflection，顶级 Reflection …（交叉进行的）
-> 通过 association（由最初的 Association 提供） 调用对应的 Association 提供的方法... （交叉进行的）
（上面两个同样的地位，前者偏向于关系，后者偏向于方法）
```

对内部实现来说非常重要的实例方法：

```
association(name)
```

通过它调用对应 Association 提供的方法。
<br>
通过它调用 CollectionProxy 提供的方法。

