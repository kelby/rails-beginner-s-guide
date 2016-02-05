
## 关于 autoload_paths 和 eager_load_paths

- 开发环境，启动速度要快（不完整也没关系）
- 生产环境，项目要完整（启动速度慢点也没关系）
- 开发环境，经常更改代码，不用重启就能运行
- 生产环境，假设不能/不会更改代码
- 旧版本使用自动加载还面临线程安全问题，在这生产环境是不能忍受的

怎么自动加载？按约定来...

```ruby
# 总开关，开发、生产环境不同
eager_load
```

```ruby
# 立即加载此路径下的文件
eager_load_paths

# 自动(延迟)加载此路径下的文件
autoload_paths
```

例外：app/ 既在 autoload_paths, 又在 eager_load_paths

什么情况下，你配置了也没用？

开发环境配置立即加载，仍然按自动加载执行。
<br>
生产环境下只配置自动加载，为了线程安全等考虑，需要再加上立即加载。

为什么不直接对 lib/ 用 eager_load_paths ？

想法：反正在开发环境是自动加载，在生产环境是立即加载。

坏处：用的是 lib/**/*.rb 模式，生产环境会把多余的 tasks、generators 也立即加载了。

require_dependency 如何使用？

initializers 和 tasks 只执行一次。

使用 eager_load_paths 会影响 autoload_paths。在开发环境，等同于使用 autoload_paths；在生产环境等同于它自己。

使用 autoload_paths 并不会影响 eager_load_paths

Rails 3、Rails 4 使用 autoload_paths 在生产环境也能工作，但会存循环依赖等风险，偶尔会出问题。

Rails 5 强制使用 eager_load_paths
