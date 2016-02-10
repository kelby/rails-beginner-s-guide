## Security Utils

```
secure_compare
```

`a_string == b_string` 当发现两字符串有字符不一致时，就会立即返回比较结果。

在这样的机制下, 比较时间与字符串匹配度是有正比关系的, 字符串匹配度越高，比较时间越长。因此，便有可能通过对比时间差的方式逐一猜测破解(计时攻击)。

使用此方法，即使发现两字符串有不同，也不会立即返回结果，而是继续比较下去，确保比较时间是一样的，可预防计时攻击。

MessageVerifier 在校验信息 `valid_message?` 的时候就用到了它。
<br>
HttpAuthentication Basic 认证的时候就用到了它。
<br>
RequestForgeryProtection 在比较 token 的时候也用到了它。

参考

[计时攻击原理以及 Rails 对应的防范](https://ruby-china.org/topics/21380)
