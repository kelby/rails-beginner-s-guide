两部分: Message Verifier 和 Message Encryptor. 

## Message Verifier

生成加密的文本，然后用于校验。这里的加密仅意味着"加签名、防篡改"，过程是可逆的，请注意使用场景。
使用场景举例，生成"记住我"的 token，或生成"取消订阅"的链接。

```ruby
verifier = ActiveSupport::MessageVerifier.new('your-secret')
message = "String that is prevented from tampering."

# 签名(加密)
signed_message = verifier.generate(message)

# 验证(解密)
verified = verifier.verify(signed_message)

# 比较
verified == message
# => true
```

主要是 `generate(value)` 和 `verify(signed_message)`，原理很简单，这里不多赘述。

## Message Encryptor

和 Message Verifier 本质上没有区别。但使用上会更严格一点，会多一些步骤(相应地，也更安全了一点)，并且它也有调用到 Message Verifier.

使用举例:

```ruby
salt = SecureRandom.random_bytes(64)
secret_key_base = '-- secret key base --'

key_generator = ActiveSupport::KeyGenerator.new(secret_key_base)
# 生成密钥
key = key_generator.generate_key(salt)

encryptor1 = ActiveSupport::MessageEncryptor.new(key)
encryptor2 = ActiveSupport::MessageEncryptor.new(key)

message = "Encrypted string."

# 加密
encrypted_message = encryptor1.encrypt_and_sign(message)

# 解密
decrypted = encryptor2.decrypt_and_verify(encrypted_message)

# 比较
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

> Note: 注意和【Key Generator 和 Caching Key Generator】章节的联系。
