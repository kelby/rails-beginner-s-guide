## Key Generator 和 Caching Key Generator

相同点，都提供方法：

```
generate_key
```

不同点：

Key Generator 生成的是**初级**密钥。

```ruby
key_generator = ActiveSupport::KeyGenerator.new 'secret'
key_generator.generate_key 'salt'
```

Caching Key Generator 再次封装 Key Generator 和 Thread Safe，生成的是**高级**密钥。

```ruby
key_generator = ActiveSupport::KeyGenerator.new 'secret'

# 参数是 KeyGenerator 实例对象
caching_key_generator = ActiveSupport::CachingKeyGenerator.new(key_generator)
caching_key_generator.generate_key 'salt'
```

> Note: 注意和【Message Encryptor 和 Message Verifier】章节的联系。
