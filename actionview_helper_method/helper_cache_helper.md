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

