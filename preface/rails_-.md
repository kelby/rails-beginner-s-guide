由于时间关系，临时记录一点，待有时间再补充完整。

- Rails 4有哪些优点，我们可能没注意到？

这个问题问得太好了。请看  


session - Rails3 的 session 可以被 decode 得到內容，但如果 application 的 secret key 被取得就可以取得 session 的內容。Rails4 不允許 session 被 decode 這個行為，並建議 secret_token 改用環境變數。

[Rails4 學習心得整理 (上)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-1/)

[Rails4 學習心得整理 (下)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-2/)

模板 - Rails4 新增了一個 .ruby 的 template 去取代 .jbuilder。

ETag 成 middleware - Etag 主要是用來做 client side cache，browser 發出 request 時先用 headers['ETag']和 server 做比對，如果 Server 的 Etag 是 match 的，即不重新 render body 並送出 304 not modify 給 browser。

ACTIONCONTROLLER::LIVE - 類似 Websocket 的功能，實作方式為 SEE(Server Sent Events)，和 Websocket 的差別在於 Websocket 在 client 可以 push 也可以 receive，而 SEE 在 client 只能 revive。舉個例子，Websocket 很適合用在聊天室類型會 push 也會 receive 的狀況，而 SEE 適合用在類似 Twitter timelime 自動更新的部分，搭配 Redis 的 pub/sub 更是極品。

對於 Rails 來說，如果想要使用 Websocket 必須使用 em-websocket，但直接掛在 Rails 上不是很穩定，必須要獨立程式配上 80 以外的 port 才能運行，略顯麻煩。但前端有 js 及 flash，普遍支援度較高。而 SEE可以很方便直接在 Rails4 上面使用，但瀏覽器支援度很低，IE 系列幾乎全死，目前不確定有沒有其他外部方案(flash)可以支援，如果有的話拜託和我說一下。 :)

---------

Rails 4 里 respond_to 可以指定 format, format 里可以指定 variant.
