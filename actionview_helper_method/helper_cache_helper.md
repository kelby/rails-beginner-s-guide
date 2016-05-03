### Cache Helper

缓存。

```
cache
```

```
cache_if
cache_unless

cache_fragment_name
```

调用 `cache` 这个 helper 方法，生成 `cache_key` 的主要流程。

```
ActionView::Helpers::CacheHelper -> AbstractController::Caching::Fragments -> ActiveSupport::Cache
```

根据对象、数组、Relation、字符串等有不同的生成方式。

开启 `perform_caching` 并且对于静态页面 Digestor，对生成内容进行“消毒”等细节在此不做讨论，请自行查看源代码。

