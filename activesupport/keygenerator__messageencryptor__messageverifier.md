## Message Encryptor 和 Message Verifier

### Message Verifier

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

### Message Encryptor

使用类似:

```ruby
salt = SecureRandom.random_bytes(64)
secret_key_base = '-- secret key base --'

key_generator = ActiveSupport::KeyGenerator.new(secret_key_base)
key = key_generator.generate_key(salt)

encryptor1 = ActiveSupport::MessageEncryptor.new(key)
encryptor2 = ActiveSupport::MessageEncryptor.new(key)

message = "Encrypted string."

# Encrypt the message...
encrypted_message = encryptor1.encrypt_and_sign(message)
# and decrypt it.
decrypted = encryptor2.decrypt_and_verify(encrypted_message)

# Decrypted message equals the original message
decrypted == message
# => true
```

主要是 `encrypt_and_sign(value)` 和 `decrypt_and_verify(value)`，同样这里不多赘述。

Message Encryptor、Message Verifier 和 Key Generator 这三者使用类似，创建时可传递字符串做为参数，然后进行加密，生成的也是字符串。

### 实例参考

```ruby
salt  = SecureRandom.random_bytes(64) # 保存进数据库
secret_key_base = '-- secret base --' # 保存到配置文件

string = 'string'                     # 运行于代码
secret = Digest::MD5.hexdigest("#{secret_key_base}-#{string}")
key    = ActiveSupport::KeyGenerator.new(secret).generate_key(salt)
crypt1 = ActiveSupport::MessageEncryptor.new(key)

string = 'string'                     # 运行于代码
secret = Digest::MD5.hexdigest("#{secret_key_base}-#{string}")
key    = ActiveSupport::KeyGenerator.new(secret).generate_key(salt)
crypt2 = ActiveSupport::MessageEncryptor.new(key)

source_data    = 'my secret data'                     # 加密前数据
encrypted_data = crypt1.encrypt_and_sign(source_data) # 加密后数据
decode_data    = crypt2.decrypt_and_verify(encrypted_data)
source_data == decode_data
```

> Note: 注意和下一章节"Key Generator 和 Caching Key Generator"的联系
