## 定制 Middleware

[Rails on Rack](http://guides.rubyonrails.org/rails_on_rack.html)

[Where do you put your Rack middleware files and requires?](http://stackoverflow.com/questions/3428343/where-do-you-put-your-rack-middleware-files-and-requires)

[#151 Rack Middleware](http://railscasts.com/episodes/151-rack-middleware)

[Sanitizing POST params with custom Rack middleware](http://pivotallabs.com/sanitizing-post-params-with-custom-rack-middleware/)

在 app/middleware/ 目录下，按照 middleware 写，然后在 config 里 use 就行了，不用做其它的配置。

Middleware 处理的是 @app 和 env 所以也是很有限的。
