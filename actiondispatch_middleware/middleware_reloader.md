### Reloader

**保证在请求之前和响应之后做某事**。这里重点是"保证"，具体是什么事，怎么做，不由它决定。

类方法：

```
to_prepare

to_cleanup
```

`to_prepare` 保证在每次请求之前，执行它给的 block.
<br>
`to_cleanup` 保证在每次响应之后，执行它给的 block.

实例方法(middleware 的 call 方法会调用它们)：

```
prepare! # 执行 to_prepare 给的 block.

cleanup! # 执行 to_cleanup 给的 block.
```

类方法(作用同上，但我们可以手动调用)：

```
prepare!

cleanup!
```

开发环境下，我们会经常更改代码，但我们又希望请求、响应速度加快。此时，可以使用此 middleware，因为它可以帮我们达到愿望。


