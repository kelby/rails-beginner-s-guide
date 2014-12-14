## Lookup Context

**从其包含属性和行为来分析，它就是为了查找 Template.**

先看官方解释

```
LookupContext is the object responsible to hold all information required to lookup templates, i.e. view paths and details. 

The LookupContext is also responsible to generate a key, given to view paths, used in the resolver cache lookup. Since this key is generated just once during the request, it speeds up all cache accesses.
```

在【渲染的原理】里，我们就讲了 lookup context 的作用，但那只是示例程序，只讲到了其中的一点。Rails 项目远比这复杂得多，所以 lookup context 的作用也远比这重要。

**类方法**

```
register_detail
```

放到 registered_details，并生成 default_x 方法。<br>
默认已经注册有：formats、variants 和 handlers.

**lookup_context 内容**

只包含以下 7 项内容，至于怎么用不是它关心的。

```
 @cache=true,
 @details=
  {:locale=>[:en],
   :formats=>[:html, :text, :js, :css, :xml, :json],
   :variants=>[],
   :handlers=>[:erb, :builder, :raw, :ruby]},
 @details_key=nil,
 @prefixes=[],
 @rendered_format=nil,
 @skip_default_locale=false,
 @view_paths=
  #<ActionView::PathSet:0x007ff7250a0fe0
   @paths= ...
```

**lookup_context 举例**

查看应用根目录下文件及目录：

```
✗ ls
Gemfile      README.rdoc  app          blorgh       config.ru    lib          public       tmp
Gemfile.lock Rakefile     bin          config       db           log          test         vendor
```

创建一个简单的 lookup_context

```ruby
FIXTURE_LOAD_PATH = File.join(File.dirname(__FILE__), '../fixtures')
=> "./fixtures"

require 'action_view'
=> true

# 仍然只包含 7 项内容
lookup_context = ActionView::LookupContext.new(FIXTURE_LOAD_PATH, {})
=> #<ActionView::LookupContext:0x007ff725058088
 @cache=true,
 @details=
  {:locale=>[:en],
   :formats=>[:html, :text, :js, :css, :xml, :json],
   :variants=>[],
   :handlers=>[:erb, :builder, :raw, :ruby]},
 @details_key=nil,
 @prefixes=[],
 @rendered_format=nil,
 @skip_default_locale=false,
 @view_paths=
  #<ActionView::PathSet:0x007ff7250a0fe0
   @paths=
    [#<ActionView::OptimizedFileSystemResolver:0x007ff7250a0f68
      @cache=
       #<ActionView::Resolver::Cache:0x007ff7250a0f40
        @data=
         #<ActionView::Resolver::Cache::SmallCache:0x007ff7250a0ec8
          @backend={},
          @default_proc=
           #<Proc:0x007ff72526d3a0@/Users/kelby/.rvm/rubies/ruby-2.1.0/lib/ruby/gems/2.1.0/gems/actionview-4.1.0/lib/action_view/template/resolver.rb:48 (lambda)>>>,
      @path="/Users/kelby/rails4/polymorphic_url_001/fixtures",
      @pattern=
       ":prefix/:action{.:locale,}{.:formats,}{+:variants,}{.:handlers,}">]>>
```

真实情况远比这复杂，@view_paths 下有很多的内容。

### View Paths

```
exists? & template_exists?
find & find_template

find_all

view_paths=

with_fallbacks
```

`find` 很重要的方法，用来查找模板(Template)。

```
detail_args_for
```

使用举例，在视图里检查局部模板是否存在：

```
template_exists?("form", "posts", true)
```

### Details Cache

```
details_key

disable_cache
```

### DetailsKey

```
get

clear
```

---

由原来的 Template::Lookup 改名而来 LookupContext.

注意它在 Base 里的 delegate.
