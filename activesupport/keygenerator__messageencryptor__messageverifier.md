# KeyGenerator 和 MessageEncryptor 和 MessageVerifier

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
