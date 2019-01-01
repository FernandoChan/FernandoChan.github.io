---
layout: post
title: ""
subtitle: ""
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - 
typora-copy-images-to: ..\img
---



直接使用Rails提供的`resources`方法：

```ruby
  Rails.application.routes.draw do
    resources :users
  end
```

`rails route` 查看一下route命令

在加了resource以后:

```bash
           Prefix Verb   URI Pattern                       Controller#Action
 ----------------------------------------------------------------------------------------
    damagereports GET    /damagereports(.:format)          damagereports#index
                  POST   /damagereports(.:format)          damagereports#create
 new_damagereport GET    /damagereports/new(.:format)      damagereports#new
edit_damagereport GET    /damagereports/:id/edit(.:format) damagereports#edit
     damagereport GET    /damagereports/:id(.:format)      damagereports#show
                  PATCH  /damagereports/:id(.:format)      damagereports#update
                  PUT    /damagereports/:id(.:format)      damagereports#update
                  DELETE /damagereports/:id(.:format)      damagereports#destroy
       workorders GET    /workorders(.:format)             workorders#index
                  POST   /workorders(.:format)             workorders#create
    new_workorder GET    /workorders/new(.:format)         workorders#new
   edit_workorder GET    /workorders/:id/edit(.:format)    workorders#edit
        workorder GET    /workorders/:id(.:format)         workorders#show
                  PATCH  /workorders/:id(.:format)         workorders#update
                  PUT    /workorders/:id(.:format)         workorders#update
                  DELETE /workorders/:id(.:format)         workorders#destroy
     potholelists GET    /potholelists(.:format)           potholelists#index
                  POST   /potholelists(.:format)           potholelists#create
  new_potholelist GET    /potholelists/new(.:format)       potholelists#new
 edit_potholelist GET    /potholelists/:id/edit(.:format)  potholelists#edit
      potholelist GET    /potholelists/:id(.:format)       potholelists#show
                  PATCH  /potholelists/:id(.:format)       potholelists#update
                  PUT    /potholelists/:id(.:format)       potholelists#update
                  DELETE /potholelists/:id(.:format)       potholelists#destroy
     sessions_new GET    /sessions/new(.:format)           sessions#new
  sessions_create POST   /sessions/create(.:format)        sessions#create
   applicants_new GET    /applicants/new(.:format)         applicants#new
applicants_create POST   /applicants/create(.:format)      applicants#create
             root GET    /                                 sessions#new
            posts GET    /posts(.:format)                  posts#index
                  POST   /posts(.:format)                  posts#create
         new_post GET    /posts/new(.:format)              posts#new
        edit_post GET    /posts/:id/edit(.:format)         posts#edit
             post GET    /posts/:id(.:format)              posts#show
                  PATCH  /posts/:id(.:format)              posts#update
                  PUT    /posts/:id(.:format)              posts#update
                  DELETE /posts/:id(.:format)              posts#destroy
```

前面的Prefix 是干嘛的:

后面接上`_path`或`_url`后可以变成「产生相对应的路径或网址」的View Helper。如果是站内连结，通常会使用`_path`写法来产生站内的路径，例如：

```
products + path     = products_path         => /products
new_product + path  = new_product_path      => /products/new
edit_product + path = edit_product_path(2)  => /products/2/edit
```

如果是使用`_url`则会产生完整的路径，包括主机网域名称：

```
products + url     = products_url         => http://kaochenlong.com/products
new_product + url  = new_product_url      => http://kaochenlong.com/products/new
edit_product + url = edit_product_url(2)  => http://kaochenlong.com/products/2/edit
```

resource 后面是复数: 这是一个rails惯例, 比如在phtrs例子里面

```ruby
Rails.application.routes.draw do
  resources :damagereports
  resources :workorders
  resources :potholelists
```

## 如果只要CRUD路由方法的一部分

```ruby
  Rails.application.routes.draw do
    resources :products, only: [:index, :show]

    # 或是反過來這樣寫也行
    # resources :products, except: [:new, :create, :edit, :update, :destroy]
  end
```



```

```







