ActionView::Helpers::CacheHelper#cache
```
  cache_fragment_name
    fragment_name_with_digest
          names  = Array(name.is_a?(Hash) ? controller.url_for(name).split("://").last : name)
          digest = Digestor.digest name: @virtual_path, finder: lookup_context, dependencies: view_cache_dependencies

          [ *names, digest ]

  fragment_for
    read_fragment_for
      controller.read_fragment
    write_fragment_for
      controller.write_fragment
```

ActionController::Caching::Fragments

```
Fragment caching is used for caching various blocks within
views without caching the entire action as a whole. This is
useful when certain elements of an action change frequently or
depend on complicated state while other parts rarely change or
can be shared amongst multiple parties. The caching is done using
the +cache+ helper available in the Action View. See
ActionView::Helpers::CacheHelper for more information.

While it's strongly recommended that you use key-based cache
expiration (see links in CacheHelper for more information),
it is also possible to manually expire caches. For example:

  expire_fragment('name_of_cache’)

当然，除了上述方法外还有：
fragment_cache_key(key)
fragment_exist?(key, options = nil)

read_fragment(key, options = nil)
write_fragment(key, content, options = nil)
```

ActionController::Caching::ConfigMethods#cache_store

```
\Caching is a cheap way of speeding up slow applications by keeping the result of
calculations, renderings, and database calls around for subsequent requests.

You can read more about each approach by clicking the modules below.

Note: To turn off all caching, set
  config.action_controller.perform_caching = false

== \Caching stores

All the caching stores from ActiveSupport::Cache are available to be used as backends
for Action Controller caching.

Configuration examples (MemoryStore is the default):

  config.action_controller.cache_store = :memory_store
  config.action_controller.cache_store = :file_store, '/path/to/cache/directory'
  config.action_controller.cache_store = :mem_cache_store, 'localhost'
  config.action_controller.cache_store = :mem_cache_store, Memcached::Rails.new('localhost:11211')
  config.action_controller.cache_store = MyOwnStore.new('parameter’)
```

Rails::Application::Configuration

```
  attr_accessor :cache_store

  # setting ...
```
