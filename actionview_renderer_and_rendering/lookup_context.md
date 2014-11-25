## Lookup Context

**类方法**

```
register_detail
```

默认

```ruby
register_detail(:locale) do
  locales = [I18n.locale]
  locales.concat(I18n.fallbacks[I18n.locale]) if I18n.respond_to? :fallbacks
  locales << I18n.default_locale
  locales.uniq!
  locales
end
register_detail(:formats) { ActionView::Base.default_formats || [:html, :text, :js, :css,  :xml, :json] }
register_detail(:variants) { [] }
register_detail(:handlers){ Template::Handlers.extensions }
```

`ActionView::LookupContext.registered_details` 查看。

**其它相关方法**

```ruby
delegate :lookup_context, :to => :view_renderer
delegate :formats, :formats=, :locale, :locale=, :view_paths, :view_paths=, :to => :lookup_context
```

**lookup_context 内容**

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
✗ pry
pry(main)> FIXTURE_LOAD_PATH = File.join(File.dirname(__FILE__), '../fixtures')
=> "./fixtures"
pry(main)> require 'action_view'
=> true
pry(main)> lookup_context = ActionView::LookupContext.new(FIXTURE_LOAD_PATH, {})
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

```
detail_args_for
```

使用举例，在视图里检查局部模板是否存在：

```
template_exists?("form", "posts", true)
```

### Details Cache

```
disable_cache
```

