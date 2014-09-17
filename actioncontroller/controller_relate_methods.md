## Controller 给 action 使用的方法(或变量)

Controller 提供了一些和环境有关的方法给 action 使用(当然，在 View 里你也可以直接或间接使用)，通过这些方法，你可以直接获取 URL 或 request 信息。这些方法包括但不限于：

- action_name:
the name of the action currently being processed.

- cookies:
the cookies associated with the request, and setting values into this object stores cookies on the browser when the response is sent.

- headers:
a hash of HTTP headers that will be used in the response.

- params:
a hash-like object containing request parameters (along with pseudopa- rameters generated during routing).

- request:
the incoming request object.

- response:
the response object, filled in during the handling of the request, normally managed for you by Rails.

- session:
a hash-like object representing the current session data.
logger
