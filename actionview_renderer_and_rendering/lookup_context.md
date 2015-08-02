## Lookup Context

**包含了一些基本信息，用于查找模板。**

由原来的 Template::Lookup 更改而来，其实名字叫 Lookup Template Context 反正更合适。

### lookup_context 内容

基本信息，包括以下 7 项内容：

```ruby
def initialize(view_paths, details = {}, prefixes = [])
  @details, @details_key = {}, nil
  @skip_default_locale = false
  @cache = true
  @prefixes = prefixes
  @rendered_format = nil

  self.view_paths = view_paths
  initialize_details(details)
end

# 也就是：

@cache=true,
@details= { ... },
@details_key=nil,
@prefixes=[],
@rendered_format=nil,
@skip_default_locale=false,
@view_paths= #<ActionView::PathSet:0x007ff7250a0fe0 ...
```

在 ActionView::Base 里有：

```ruby
delegate :formats, :formats=, :locale, :locale=, :view_paths, :view_paths=,
         :to => :lookup_context
```

### lookup_context 举例

查看应用根目录下文件及目录：

```
✗ ls
Gemfile       README.rdoc  app  blorgh  config.ru  lib  public  tmp
Gemfile.lock  Rakefile     bin  config  db         log  test    vendor
```

创建一个简单的 lookup_context

```ruby
FIXTURE_LOAD_PATH = File.join(File.dirname(__FILE__), '../fixtures')
# => "./fixtures"

require 'action_view'
# => true

# 仍然只包含 7 项内容
lookup_context = ActionView::LookupContext.new(FIXTURE_LOAD_PATH, {})
=> #<ActionView::LookupContext:0x007ff725058088
 @cache=true,
 @details= { ... },
 @details_key=nil,
 @prefixes=[],
 @rendered_format=nil,
 @skip_default_locale=false,
 @view_paths= #<ActionView::PathSet:0x007ff7250a0fe0 ...
```

真实情况远比这复杂，`@view_paths` 下有很多的内容。

### 实例方法

```
attr_accessor :prefixes, :rendered_format
mattr_accessor :fallbacks
mattr_accessor :registered_details

formats=

skip_default_locale!

locale
locale=

with_layout_format 
```

### 类方法

```
register_detail
```

放到变量 registered_details 里，并生成 default_x 等方法。

```ruby
# 默认已经注册有：locale、formats、variants 和 handlers.

register_detail(:locale) { ... }
register_detail(:formats) { ... }
register_detail(:variants) { ... }
register_detail(:handlers) { ... }
```
