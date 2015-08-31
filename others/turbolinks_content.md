## Turbolinks 3

#### Ruby 层面：

	Controller
		3 个回调方法：设置 X-XHR-Redirected-To，只对 GET 请求有效，某种情况的跨域是不允许的
		（用到了 X-XHR-Referer）
		重写 referer 方法(用到了 X-XHR-Referer )
		更改 redirect_to 计算规则（用到了 X-XHR-Referer ）
		重写 redirect_to 方法，某种情况下使用 Turbolinks.visit 代替
		重写 render 方法，某种情况下使用 Turbolinks.replace 代替
		render 和 redirect_to 可以额外处理参数：turbolinks，keep，change，flush
		（各个参数对应功能可以查看文档）
	Router
		加上 _turbolinks_redirect_to（用到了 HTTP_X_XHR_REFERER ）
	View
		加上 X-XHR-Referer

	共引入标记
		cookies[:request_method]
		request.headers['X-XHR-Referer']
		request.headers["X-XHR-Referer"]
		session[:_turbolinks_redirect_to]
		response.headers['X-XHR-Redirected-To']
		env['HTTP_X_XHR_REFERER']
		env['rack.session'][:_turbolinks_redirect_to]
		controller.request.headers["X-XHR-Referer"]

#### JS 层面：

	定义了几个变量
	有哪些事件？
	fetch 是如何实现的？
	配置要不要缓存
	fetch 核心部分之...（实际上是XMLHttpRequest对象；也用到了 X-XHR-Referer）
	fetchHistory
	cacheCurrentPage
	如何缓存、如何清除缓存？
	replace
	fetch, replace 核心之...（如何才能快速且有效的替换？fetchReplacement）
	window.history 记录历史链接、重定向链接、当前链接和状态
	clone
	各种乱七八糟的响应格式
	过期
	CSRFToken
	核心之...createDocument
	进度条
		也可以做得很复杂
	进度条之... API
	其它：ua，requestMethodIsSafe，browserSupportsTurbolinks，browserSupportsCustomEvents
	@Turbolinks 之 API

#### 文档：

	10 个事件
		整个 load 流程
		整个 replace 流程
	举例说明可以如何使用这些事件
	页面缓存（介绍，如何工作，如何配置，如何手动调用）
	过滤缓存（配置要不要用即可，配置某个页面不使用）
	进度条（好看，并且浏览器自带的进度条由于特性没有了）（配置要不要使用，配置样式，手动调用）
	永久保存（配置即可），例如全局的侧边栏，并且只加载一次
	配置某个地方的链接不使用 turboolinks，配置除 .html 外其它后缀的链接也加 turbolinks，
	    配置不要所有 redirect_to 都使用 turbolinks 特性只在部分 Controller 单独引入使用
	允许使用老的 JS 写法 jquery.turbolinks （引入即可）
	配置是否检测 JS 或 CSS 为最近版本（假设我们重新部署，md5 变了，有时候 turbolinks 不知道）
	配置 View 里的 script 只执行一次，还是每次都执行
	手动调用，明确的指定使用 turbolinks
	局部替换（也可以配合 data-turbolinks-temporary 一起使用）... 
	    客户端：要手动调用方法；服务端：同样的，要手动调用方法
	配置（全局或针对某个请求），让浏览器不要缓存 turbolinks 请求
	客户端 API (api 是 api，事件是事件，不要搞混了)
	我们极度依赖 pushState （在这里，做为用户，我们就不要考虑了吧~~）

