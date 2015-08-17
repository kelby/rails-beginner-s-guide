## Turbolinks 产生的原因

原因：

1. HTML 5 引进了 "History interface"
2. 原来的一个 request/response 需要传递整个页面（不论你有没有做缓存）
3. Assets Precompile 将整个静态资源打包，导致 css/js 太大
4. 借鉴了 PJAX

第 1 点补充：

```javascript
interface History {
  readonly attribute long length;
  readonly attribute any state;
  void go(optional long delta);
  void back();
  void forward();
  void pushState(any data, DOMString title, optional DOMString? url = null);
  void replaceState(any data, DOMString title, optional DOMString? url = null);
};
```

第 2 点补充：

```coffee
request ->
response(title and body) <-

request ->
response(only body) <-
```

类似 AJAX 但 url 变了。

第 3 点补充：

有人说 Turblinks 就是为了解决 Assets Precompile 引起的问题。

第 4 点补充：

```javascript
$.pjax({
  url: '/authors',
  container: '#main'
})
```

PJAX 是指定 dom，而 Turblinks 是整个 body.
