# ActionController

## Metal 是重点

## Metal 之外


### cache


### 继承 AbstractController 的遗产

直接继承而来

### 使用 ActionDispatch 的资源

有一些方法是 ActionDispatch::Http 直接调用

### 协作 ActionView

有很多类似的方法或丝丝关联。

![Action Dispatcher and Action Controller in Rails 4](http://dylanninin.com/assets/images/2013/rails/rails_mvc_c.png)

Controller Environment
The controller sets up the environment for actions (and, by extension, for the views that they invoke). Many of these methods provide direct access to infor- mation contained in the URL or request.

action_name: the name of the action currently being processed.
cookies: the cookies associated with the request, and setting values into this object stores cookies on the browser when the response is sent.
headers: a hash of HTTP headers that will be used in the response.
params: a hash-like object containing request parameters (along with pseudopa- rameters generated during routing).
request: the incoming request object.
response: the response object, filled in during the handling of the request, normally managed for you by Rails.
session: a hash-like object representing the current session data.
logger
