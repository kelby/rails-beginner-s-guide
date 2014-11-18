## Key Generator 和 Caching Key Generator

相同点，都提供方法：

```
generate_key
```

不同点：

Key Generator 生成的是初级密钥。

```ruby
key_generator = ActiveSupport::KeyGenerator.new 'secret'
key_generator.generate_key 'salt'
```

Caching Key Generator 相当于再次封装 KeyGenerator 和 ThreadSafe，生成的是高级密钥。

```ruby
key_generator = ActiveSupport::KeyGenerator.new 'secret'

# 参数是 key_generator
caching_key_generator = ActiveSupport::CachingKeyGenerator.new(key_generator)
caching_key_generator.generate_key 'salt'
```

> Note: 注意和上一章节"Message Encryptor 和 Message Verifier"的联系
