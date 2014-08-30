由于时间关系，临时记录一点，待有时间再补充完整。

- form_tag 和 form_for 的区别？
前者必需对应着 model 对象，后者就是普通的表单。


- 能和我说说 delegate 是怎么实现的吗？
http://ruby-windy.iteye.com/blog/1195963

- Rails 4有哪些优点，我们可能没注意到？

这个问题问得太好了。请看<br>
eager_load - load all code before new threads are created.<br>
scopes - 一律使用proc object取代原本model內的scope的參數，proc object或block取代原本model內的default_scope的參數。因為Model被cache的關係，會導致很多eager-load的scope只有在第一次載入的時候讀取正確的值(ex:時間)，這是Rails2和Rails3令人詬病的問題，而Rails4規定凡是eager-load的scope一律要使用proc object。
none - 新增了.none去取代空陣列[]避免多餘的判斷，這東西非常實用。<br>
not - 新增了.not去產生"IS NOT NULL"的query，這東西非常實用。<br>
order - Rails4終於把default_scope的order調整成正常的順序，default_scope的order永遠會在最後而不像Rails3優先權永遠是最高的。同時order可以使用hash帶入，也不需要再用字串了。<br>
references - 使用includes的時候會有參照table的問題，Rails4新增了一個references去明確指出參照的table，但如果在where的參數內是直接用hash的conditions，即可不用指定referen<br>
session - Rails3的session可以被decode得到內容，但如果application的secret key被取得就可以取得session的內容。Rails4不允許session被decode這個行為，並建議secret_token改用環境變數。

[Rails4 學習心得整理 (上)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-1/)
[Rails4 學習心得整理 (下)](http://blog.hellolucky.info/articles/ruby-on-rails-rails4-learning-experience-finishing-rails-4-zombie-outlaws-2/)

form helper - 加上了collection_radio_buttons和collection_check_boxes這兩個helper，期待很久的功能。
data field - 以前使用date_select總是會產生多個input去區分年、月、日…，Rails4回歸正常行為，使用date_select就只會產生一個input。
模板 - Rails4新增了一個.ruby的template去取代.jbuilder。
ETag 成 middleware - Etag主要是用來做client side cache，browser發出request時先用headers['ETag']和server做比對，如果Server的Etag是match的，即不重新render body並送出304 not modify給browser。
ACTIONCONTROLLER::LIVE - 類似Websocket的功能，實作方式為SEE(Server Sent Events)，和Websocket的差別在於Websocket在client可以push也可以receive，而SEE在client只能revive。舉個例子，Websocket很適合用在聊天室類型會push也會receive的狀況，而SEE適合用在類似Twitter timelime自動更新的部分，搭配Redis的pub/sub更是極品。

對於Rails來說，如果想要使用Websocket必須使用em-websocket，但直接掛在Rails上不是很穩定，必須要獨立程式配上80以外的port才能運行，略顯麻煩。但前端有js及flash，普遍支援度較高。而SEE可以很方便直接在Rails4上面使用，但瀏覽器支援度很低，IE系列幾乎全死，目前不確定有沒有其他外部方案(flash)可以支援，如果有的話拜託和我說一下。 :)


- 对于 Association 你还有什么要说的？
http://jonathanhui.com/ruby-rails-3-model-association

- 对 Association 里的 has_many 多讲解一点？
http://depthcoding.ybarthelemy.info/2012/07/30/under-the-hoods-of-activerecords-has_many-method/

- 经验谈？
http://blog.hellolucky.info/articles/ruby-on-rails-large-amounts-of-data-transfer-json-the-use-json-generate-data/

---------

Rails 4 里 respond_to 可以指定 format, format 里可以指定 variant.

---------

We want to generate the methods via module_eval rather than
define_method, because define_method is slower on dispatch.
Evaluating many similar methods may use more memory as the instruction
sequences are duplicated and cached (in MRI).  define_method may
be slower on dispatch, but if you're careful about the closure
created, then define_method will consume much less memory.

But sometimes the database might return columns with
characters that are not allowed in normal method names (like
'my_column(omg)'. So to work around this we first define with
the __temp__ identifier, and then use alias method to rename
it to what we want.

We are also defining a constant to hold the frozen string of
the attribute name. Using a constant means that we do not have
to allocate an object on each call to the attribute method.
Making it frozen means that it doesn't get duped when used to
key the @attributes_cache in read_attribute.
