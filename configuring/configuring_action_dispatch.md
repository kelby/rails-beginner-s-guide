### Action Dispatch

`Rails.configuration.action_dispatch.keys`

NilClass

```
:x_sendfile_header
:default_charset
```

TrueClass

```
:ip_spoofing_check
:show_exceptions
:perform_deep_munge
:always_write_cookie
```

FalseClass

```
:ignore_accept_header
:rack_cache
```

Fixnum

```
:tld_length
```

Hash

```
:rescue_templates
:rescue_responses
:default_headers
```

String

```
:http_auth_salt
:signed_cookie_salt
:encrypted_cookie_salt
:encrypted_signed_cookie_salt
```

Symbol

```
:cookies_serializer
```

附录：

```ruby
klasses = Rails.configuration.action_dispatch.values.map(&:class).uniq

klasses.each do |klass|
  p klass
  Rails.configuration.action_dispatch.each_pair do |k,v|
    p k if v.class == klass
  end
end
```
