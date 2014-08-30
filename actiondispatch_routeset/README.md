# ActionDispatch RouteSet

## class Dispatcher

每一个路由规则转换着 `draw` 对应一个 Dispatcher 实例。

重要的转发方法 `dispatch`，将战场切换到 Controller#action

## class NamedRouteCollection

### 各个实例方法
### UrlHelper

## module MountedHelpers

## class Generator

各个实例方法

## 各个实例方法

## get call with defaults

Here we have following route

```ruby
Rails4demo::Application.routes.draw do
  get 'photos/:id' => 'photos#show', :defaults => { :format => 'jpg' }
end
```

After the normalization process the above routing statement is transformeed into five different variables. The values for all those five varibles is shown below.

```ruby
app: #<ActionDispatch::Routing::RouteSet::Dispatcher:0x007fd05e0cf7e8
           @defaults={:format=>"jpg", :controller=>"photos", :action=>"show"},
           @glob_param=nil,
           @controller_class_names=#<ThreadSafe::Cache:0x007fd05e0cf7c0
           @backend={},
           @default_proc=nil>>
conditions: {:path_info=>"/photos/:id(.:format)", :required_defaults=>[:controller, :action], :request_method=>["GET"]}
requirements: {}
defaults: {:format=>"jpg", :controller=>"photos", :action=>"show"}
as: nil
anchor: true
```

`app` is the application that will be executed if conditions are met. `conditions` are the conditions. Pay attention to `:path_info` in conditions. This is used by Rails to determine the right route statement. `defaults` are defaults and `requirements` are the constraints.

## GET call with as

Here we have following route

```ruby
Rails4demo::Application.routes.draw do
  get '/logout' => 'sessions#destroy', :as => :logout
end
```

After normalization above code gets following values

```ruby
app: #<ActionDispatch::Routing::RouteSet::Dispatcher:0x007f8ded87e740
           @defaults={:controller=>"sessions", :action=>"destroy"},
           @glob_param=nil,
           @controller_class_names=#<ThreadSafe::Cache:0x007f8ded87e718 @backend={},
           @default_proc=nil>>
conditions: {:path_info=>"/logout(.:format)", :required_defaults=>[:controller, :action], :request_method=>["GET"]}
requirements: {}
defaults: {:controller=>"sessions", :action=>"destroy"}
as: "logout"
anchor: true
```

Notice that in the above case as is populate with `logout` .

### root call

Here we have following route

```ruby
Rails4demo::Application.routes.draw do
  root 'users#index'
end
```

After normalization above code gets following values

```ruby
app: #<ActionDispatch::Routing::RouteSet::Dispatcher:0x007fe91507f278
           @defaults={:controller=>"users", :action=>"index"},
           @glob_param=nil,
           @controller_class_names=#<ThreadSafe::Cache:0x007fe91507f250 @backend={},
           @default_proc=nil>>
conditions: {:path_info=>"/", :required_defaults=>[:controller, :action], :request_method=>["GET"]}
requirements: {}
defaults: {:controller=>"users", :action=>"index"}
as: "root"
anchor: true
```

Notice that in the above case `as` is populated. And the `path_info` is `/` since this is the root url .

## get with a redirect

Here we have following route

```ruby
Rails4demo::Application.routes.draw do
  get "/stories" => redirect("/posts")
end
```

After normalization above code gets following values

```ruby
app: redirect(301, /posts)
conditions: {:path_info=>"/stories(.:format)", :required_defaults=>[], :request_method=>["GET"]}
requirements: {}
defaults: {}
as: "stories"
anchor: true
```

Notice that in the above case app is a simple redirect .

---

### How to find the matching route definition

So now that we have normalized the routing definitions the task at hand is to find the right route definition for the given url along with request_method.

For example if the requested page is `/pictures/A12345` then the matching routing definition should be `get 'pictures/:id' => 'pictures#show', :constraints => { :id => /[A-Z]\d{5}/ }` .

In order to accomplish that I would do something like this.

I would convert all path info into a regular experssion and I would push that regular expression in an array. So in this case I would have 12 regular expressions in the array and for the given url I would try to match one by one.

This strategy will work and this is how Rails worked all the way upto Rails 3.1 .

### Aaron Patterson loves computer science

[Aaron Patterson](http://twitter.com/tenderlove) noticed that finding the best matching route definition for a given url is nothing else but pattern matching task. And computer science solved this problem much more elegantly and this happens to run faster also by building an AST and walking over it.

So he decided to make a mini language out of the route definitions . After all the route definitions , we write , follow certain rules.

And thus [Journey](http://github.com/rails/journey) was born.

In the next blog we will see how to write grammar rules for routing definitions , how to parse and then walk the ast to see the best match .