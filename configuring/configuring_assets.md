### Assets

```ruby
# 配置静态资源所在网址。我们网站所用的静态资源可以单独存放
# CDN、网站对静态资源访问（并发）比较大的时候可以考虑
config.asset_host

# 是否使用 Assets Pipeline, 默认为 true
config.assets.enabled

config.assets.raise_runtime_errors

config.assets.compress

config.assets.css_compressor
# 如：config.assets.css_compressor = :yui

config.assets.js_compressor
# 如：config.assets.js_compressor = :uglifier

config.assets.paths

config.assets.precompile

config.assets.prefix

config.assets.manifest

config.assets.digest

config.assets.debug

config.assets.cache_store

config.assets.version

config.assets.compile

config.assets.logger
```
