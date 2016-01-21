## Connection

和 Channel 平级，但又被其调用。

### Identification

```
connection_identifier

identified_by
```

常用 `identified_by` 进行身份验证。

### Authorization

```
reject_unauthorized_connection
```

常用 `reject_unauthorized_connection` 进行报错，只能被动地等着被调用。
