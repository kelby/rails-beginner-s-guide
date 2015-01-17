## Head

**返回一个内容为空的 response.**

有时候(作为 Web service 时)，响应内容可能只需要一个状态码，其它内容都不需要。

直接用 `head` 方法:

```ruby
head :created, :location => person_url(@person)
```

注意：使用 head 只是设置了 respond_body，如果程序还没有结束，那么 Controller#action 层面的后续代码还是会执行的。

结果和使用 render nothing: true 类似，所以你也可以这么做：

```ruby
headers['Location'] = person_url(@person)
render :nothing => true, :status => "201 Created"
```

当然，如果有多个地方使用到此功能的话，render 写起来还是挺让人头疼的。

**附：对照表**

|Response Class | HTTP Status Code |	Symbol |
|--|--|--|
| 消息 |	100|	:continue|
||101 |	:switching_protocols |
||102 |	:processing |
| 成功 |	200 |	:ok
||201 |	:created |
| |202 |	:accepted |
| |203 |	:non_authoritative_information |
| |204 |	:no_content |
| |205 |	:reset_content |
| |206 |	:partial_content |
| |207 |	:multi_status |
| |208 |	:already_reported |
| |226 |	:im_used |
| 重定向 |	300 |	:multiple_choices
| |301 |	:moved_permanently |
| |302 |	:found |
| |303 |	:see_other |
| |304 |	:not_modified |
| |305 |	:use_proxy |
| |306 |	:reserved |
| |307 |	:temporary_redirect |
| |308 |	:permanent_redirect |
| 客户端错误 |	400	| :bad_request
||401 |	:unauthorized |
||402 |	:payment_required |
||403 |	:forbidden |
||404 |	:not_found |
||405 |	:method_not_allowed |
||406 |	:not_acceptable |
||407 |	:proxy_authentication_required |
||408 |	:request_timeout |
||409 |	:conflict |
||410 |	:gone |
||411 |	:length_required |
||412 |	:precondition_failed |
||413 |	:request_entity_too_large |
||414 |	:request_uri_too_long |
||415 |	:unsupported_media_type |
||416 |	:requested_range_not_satisfiable |
||417 |	:expectation_failed |
||422 |	:unprocessable_entity |
||423 |	:locked |
||424 |	:failed_dependency |
||426 |	:upgrade_required |
||428 |	:precondition_required |
||429 |	:too_many_requests |
||431 |	:request_header_fields_too_large |
| 服务器错误 |	500	 | :internal_server_error
||501 |	:not_implemented |
||502 |	:bad_gateway |
||503 |	:service_unavailable |
||504 |	:gateway_timeout |
||505 |	:http_version_not_supported |
||506 |	:variant_also_negotiates |
||507 |	:insufficient_storage |
||508 |	:loop_detected |
||510 |	:not_extended |
||511 |	:network_authentication_required |
