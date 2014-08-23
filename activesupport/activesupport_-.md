# ActiveSupport - 一些模板的资料
## ActiveSupport::TimeWithZone

类似 Ruby 内置的 [Time](http://ruby-doc.org/core-2.1.0/Time.html)，但 Ruby 内置的 Time 所创建的实例对象局限于 UTC 和系统的 ENV['TZ'] 时区。这里的实例没有此局限，你可以用你想用的时区。

你不能直接使用 new 创建 TimeWithZone 实例对象，但可以用 local, parse, at 和 now 创建 `TimeZone` 实例对象，或者 in_time_zone 创建 `Time` 和 `DateTime` 实例对象。

```ruby
Time.zone = 'Eastern Time (US & Canada)'        # => 'Eastern Time (US & Canada)'
Time.zone.local(2007, 2, 10, 15, 30, 45)        # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.parse('2007-02-10 15:30:45')          # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.at(1170361845)                        # => Sat, 10 Feb 2007 15:30:45 EST -05:00
Time.zone.now                                   # => Sun, 18 May 2008 13:07:55 EDT -04:00
Time.utc(2007, 2, 10, 20, 30, 45).in_time_zone  # => Sat, 10 Feb 2007 15:30:45 EST -05:00
```

查询 Time 和 TimeZone 的 API 可以对这些方法有更多的了解。

TimeWithZone 创建的实例对象和 Ruby 内置的 Time 创建的实例对象完全兼容，也就是说它们是等价关系。

```ruby
t = Time.zone.now                     # => Sun, 18 May 2008 13:27:25 EDT -04:00
t.hour                                # => 13
t.dst?                                # => true
t.utc_offset                          # => -14400
t.zone                                # => "EDT"
t.to_s(:rfc822)                       # => "Sun, 18 May 2008 13:27:25 -0400"
t + 1.day                             # => Mon, 19 May 2008 13:27:25 EDT -04:00
t.beginning_of_year                   # => Tue, 01 Jan 2008 00:00:00 EST -05:00
t > Time.utc(1999)                    # => true

# 完全兼容，它们是等价关系。
t.is_a?(Time)                         # => true
t.is_a?(ActiveSupport::TimeWithZone)  # => true
```

相关：Date, Time, DateTime, DateAndTime 以及 TimeZone

## TaggedLogging

封装了 Ruby 标准的 [Logger](http://ruby-doc.org/stdlib-2.1.0/libdoc/logger/rdoc/index.html)，让我们能够给 logger 实例对象打'标签'。

```ruby
    logger = ActiveSupport::TaggedLogging.new(Logger.new(STDOUT))
    logger.tagged('BCX') { logger.info 'Stuff' }                            # Logs "[BCX] Stuff"
    logger.tagged('BCX', "Jason") { logger.info 'Stuff' }                   # Logs "[BCX] [Jason] Stuff"
    logger.tagged('BCX') { logger.tagged('Jason') { logger.info 'Stuff' } } # Logs "[BCX] [Jason] Stuff"
```

Rails.logger 默认已经使用。

实现：封装了 Logger 的实例对象，覆盖了它自带的 [Formatter](http://ruby-doc.org/stdlib-2.1.0/libdoc/logger/rdoc/Logger/Formatter.html)对象，并添加了 tagged 方法。

## StringInquirer

```ruby
Rails.env == 'production'
```

语法糖

```ruby
Rails.env.production?
```

实现原理：method_missing 然后检查方法最后是不是以 ? 结尾，再用 == 判断。
评价：真的只是语法糖，并且局限于 Rails.env 对象，所以实际意义不大。

## 扩展 Hash

可以把 Hash 的 key 当做方法，可以读写 value。`OrderedOptions` 扩展于 Hash

普通的 Hash 是这样:

```ruby
  h = {}
  h[:boy] = 'John'
  h[:girl] = 'Mary'
  h[:boy]  # => 'John'
  h[:girl] # => 'Mary'
```

使用 OrderedOptions, Hash 的 key 可以当做方法，读写 value:

```ruby
  h = ActiveSupport::OrderedOptions.new
  h.boy = 'John'
  h.girl = 'Mary'
  h.boy  # => 'John'
  h.girl # => 'Mary'
```

默认 Hash 的 `keys` 方法，是没有排序的，使用 `OrderedHash` 后会进行排序

```ruby
oh = ActiveSupport::OrderedHash.new
oh[:a] = 1
oh[:c] = 3
oh[:b] = 2
oh.keys # => [:a, :b, :c]
# Hash 默认的 keys 没有排序，这里显示的是 [:a, :c, :b]
```

实现：语法糖。

## Notifications

怎么使用？

- subscribe - ask to receive instrument data
- instrument - signal event or track data about operation(duration/exceptions)

```ruby
# 发布
ActiveSupport::Notifications.instrument "my.custom.event", this: :data do
  # do your custom stuff here
end

# 订阅
ActiveSupport::Notifications.subscribe "my.custom.event" do |name, started, finished, unique_id, data|
  puts data.inspect # {:this=>:data}
end
```

为什么使用？

Rails 内外，都有很多类似、可替换的技术。但这个方法，即能保证在同一项目内进行，又能使耦合度尽可能的降低。并且，它使用范围很广。

缺点：配置不方便，一启动就执行，并且更改要重启。默认对性能是有影响的，subscribe 并不是异步执行。

Rails 默认有很多 Instrumentation，你可以不写 instrument 直接 subscribe 它们。或者，你自己写 instrument，然后 subscribe。

控制台里运行 `ActiveSupport::Notifications.instrumenter` 可以查看有哪些 Instrumentation

Why would you do this yourself?

- Non-invasive and lightweight.
- No manipulations.
- Everything in one place.

实现：运用中间变量 notifier

> **Note:** 注意使用场景。这里并不是严格的发布者、订阅者模型，你从它们的方法名及默认参数就应该知道。

相关：
[Pssst... your Rails application has a secret to tell you](http://signalvnoise.com/posts/3091-pssst-your-rails-application-has-a-secret-to-tell-you)
[Digging Deep with ActiveSupport::Notifications](https://speakerdeck.com/nextmat/digging-deep-with-activesupportnotifications)
[#249 Notifications in Rails 3](http://railscasts.com/episodes/249-notifications-in-rails-3)
[ActiveSupport::Notifications, statistics and using facts to improve your site](http://www.reinteractive.net/posts/141-activesupport-notifications-statistics-and-using-facts-to-improve-your-site)
[Active Support Instrumentation](http://edgeguides.rubyonrails.org/active_support_instrumentation.html)

## MessageVerifier & MessageEncryptor

**MessageVerifier**

生成加密的文本，然后用于校验。这里的加密仅意味着"加签名、防篡改"，过程是可逆的，请注意使用场景。
使用场景举例，生成"记住我"的 token，或生成"取消订阅"的链接。

```ruby
verifier = ActiveSupport::MessageVerifier.new('your-secret')
message = "String that is prevented from tampering."

# Sign the message...
signed_message = verifier.generate(message)
# and verify it.
verified = verifier.verify(signed_message)

# Verified message equals the original message
verified == message #=> true
```

主要是 `generate(value)` 和 `verify(signed_message)`，原理很简单，这里不多赘述。

**MessageEncryptor**

works in similar way:

```ruby
salt = SecureRandom.random_bytes(64)
key = ActiveSupport::KeyGenerator.new('password').generate_key(salt)
encryptor = ActiveSupport::MessageEncryptor.new(key)

message = "Encrypted string."

# Encrypt the message...
encrypted_message = encryptor.encrypt_and_sign(message)
# and decrypt it.
decrypted = encryptor.decrypt_and_verify(encrypted_message)

# Decrypted message equals the original message
decrypted == message #=> true
```

主要是 `encrypt_and_sign(value)` 和 `decrypt_and_verify(value)`，同样这里不多赘述。

## Gzip

A convenient wrapper for the [zlib](http://ruby-doc.org/stdlib-2.1.0/libdoc/zlib/rdoc/index.html) standard library that allows
compression/decompression of strings with gzip.

```ruby
  gzip = ActiveSupport::Gzip.compress('compress me!')
  # => "\x1F\x8B\b\x00o\x8D\xCDO\x00\x03K\xCE\xCF-(J-.V\xC8MU\x04\x00R>n\x83\f\x00\x00\x00"

  ActiveSupport::Gzip.decompress(gzip)
  # => "compress me!"
```
