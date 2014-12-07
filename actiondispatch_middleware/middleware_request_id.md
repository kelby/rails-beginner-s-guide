**Request Id**

使用现成(请求里面有的话)，或为每个请求生成唯一的 X-Request-Id.

可以在防火墙、负载均衡、Web服务器之间传递，可用于跟踪用户，方便日志处理；或查看请求在集群中哪台机器上处理。

Rails 里可通过 `request.uuid` 查看，相关 HTTP header 是 X-Request-Id 标识。

根据经验，实际项目中可以考虑将此 middleware 移除。
