## Digestor

```
digest
```

它被 Cache Helper 调用。(也就是说调用 cache helper 方法时，会调用 到它)

此外，Action Controller 里的 EtagWithTemplateDigest 也会调用到它。(可配置将 digest 的结果放到 etaggers 里)

其它方法

```
dependencies

nested_dependencies
```

`dependencies` 会用到 Dependency Tracker
