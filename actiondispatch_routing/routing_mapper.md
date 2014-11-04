## Mapper

**一个罗卜一个坑**

一个路由

```
resources :aas
```

三部分：

1 最里面 Controller#action

```
Controller#action
aas#index
aas#create
aas#new
aas#edit
aas#show
aas#update
aas#update
aas#destroy
```

2 中间 helper 方法

```
 Prefix Verb  
    aas GET   
        POST  
 new_aa GET   
edit_aa GET   
     aa GET   
        PATCH 
        PUT   
        DELETE
```

3 最外面网址

```
Verb   URI Pattern            
GET    /aas(.:format)         
POST   /aas(.:format)         
GET    /aas/new(.:format)     
GET    /aas/:id/edit(.:format)
GET    /aas/:id(.:format)     
PATCH  /aas/:id(.:format)     
PUT    /aas/:id(.:format)     
DELETE /aas/:id(.:format)     
```

默认，它们是一一对应着的。

有时候，我们也会手动更改对应关系。
