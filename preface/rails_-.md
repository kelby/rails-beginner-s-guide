由于时间关系，临时记录一点，待有时间再补充完整。

- 能和我说说 delegate 是怎么实现的吗？
http://ruby-windy.iteye.com/blog/1195963

- Rails 4有哪些优点，我们可能没注意到？

这个问题问得太好了。请看  
eager_load - 在创建新进程之前加载所有代码  

scopes - 一律使用 proc object 取代原本model內的 scope 的參數，proc object 或 block 取代原本 model 內的 default_scope 的參數。因為 Model 被 cache 的關係，會導致很多 eager-load 的 scope 只有在第一次載入的時候讀取正確的值(ex:時間)，這是 Rails2 和 Rails3 令人詬病的問題，而 Rails4 規定凡是 eager-load 的 scope 一律要使用 proc object。  

none - 新增了 .none 去取代空陣列[]避免多餘的判斷，這東西非常實用。  

not - 新增了 .not 去產生"IS NOT NULL"的 query，這東西非常實用。

order - Rails4 終於把  default_scope 的 order 調整成正常的順序，default_scope 的 order 永遠會在最後而不像 Rails 3 優先權永遠是最高的。同時 order 可以使用 hash 帶入，也不需要再用字串了。<br>

references - 使用 includes 的時候會有參照 table 的問題，Rails4 新增了一個 references 去明確指出參照的 table，但如果在 where 的參數內是直接用 hash 的 conditions，即可不用指定 referen<br>

session - Rails3 的 session 可以被 decode 得到內容，但如果 application 的 secret key 被取得就可以取得 session 的內容。Rails4 不允許 session 被 decode 這個行為，並建議 secret_token 改用環境變數。

[Rails4 學習心得整理 (上)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-1/)

[Rails4 學習心得整理 (下)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-2/)

form helper - 加上了 collection_radio_buttons 和 collection_check_boxes 這兩個 helper，期待很久的功能。

data field - 以前使用 date_select 總是會產生多個 input 去區分年、月、日…，Rails4 回歸正常行為，使用 date_select 就只會產生一個 input。

模板 - Rails4 新增了一個 .ruby 的 template 去取代 .jbuilder。

ETag 成 middleware - Etag 主要是用來做 client side cache，browser 發出 request 時先用 headers['ETag']和 server 做比對，如果 Server 的 Etag 是 match 的，即不重新 render body 並送出 304 not modify 給 browser。

ACTIONCONTROLLER::LIVE - 類似 Websocket 的功能，實作方式為 SEE(Server Sent Events)，和 Websocket 的差別在於 Websocket 在 client 可以 push 也可以 receive，而 SEE 在 client 只能 revive。舉個例子，Websocket 很適合用在聊天室類型會 push 也會 receive 的狀況，而 SEE 適合用在類似 Twitter timelime 自動更新的部分，搭配 Redis 的 pub/sub 更是極品。

對於 Rails 來說，如果想要使用 Websocket 必須使用 em-websocket，但直接掛在 Rails 上不是很穩定，必須要獨立程式配上 80 以外的 port 才能運行，略顯麻煩。但前端有 js 及 flash，普遍支援度較高。而 SEE可以很方便直接在 Rails4 上面使用，但瀏覽器支援度很低，IE 系列幾乎全死，目前不確定有沒有其他外部方案(flash)可以支援，如果有的話拜託和我說一下。 :)

---------

Rails 4 里 respond_to 可以指定 format, format 里可以指定 variant.
