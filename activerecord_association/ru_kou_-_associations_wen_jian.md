## Associations 文件 - 4 个关联方法

Association 提供我们 4 个常用方法：

```
has_many
has_one
belongs_to
has_and_belongs_to_many
```

**其实现步骤：**

步骤一：


```
调用对应的 Builder，父 Builder, 顶级 Builder …（交叉进行的）
```

步骤二：

```sh
# 偏向于关联两者
调用对应的 Reflection，父 Reflection, 再父 Reflection，顶级 Reflection …（交叉进行的）

# 偏向于提供方法
通过 association（由最初的 Association 提供）调用对应的 Association 提供的方法 …（交叉进行的）
```

另，对内部实现来说非常重要的实例方法：

```
association(name)
```

通过它调用对应 Association 提供的方法。
<br>
通过它调用 CollectionProxy 提供的方法。

