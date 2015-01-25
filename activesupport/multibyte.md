## Multibyte

**使处理各种字符成为可能。**

类方法：

```
proxy_class
proxy_class=
```

### Chars

Rails 里很多字符串，都会先转换成它的实例对象，然后再处理。

实例方法：

```
capitalize

compose

decompose

downcase

grapheme_length

limit

normalize

respond_to_missing?

reverse

slice!

split

swapcase

tidy_bytes

titleize & titlecase

upcase
```

普通字符串通过 `mb_chars` 方法，可以得到 Chars 的实例对象，然后可以调用这里的实例方法。

```ruby
require 'active_support/multibyte'

class String
  def mb_chars
    ActiveSupport::Multibyte.proxy_class.new(self)
  end
end
```

> Note: Chars 还封装使用了下面的 Unicode.

### Unicode

类方法：

```
compose

decompose

downcase

in_char_class?

normalize

pack_graphemes

reorder_characters

swapcase

tidy_bytes

unpack_graphemes

upcase
```
