---
layout: post
---
使用Ghost api在ghost上 post 文章

[Ghost](https://github.com/TryGhost)简洁、优雅受到很多人喜爱，0.5版本以后，开始提供api，方便第三方从站外调用Ghost的功能，目前api处于开发状态（虽然可以调用），还是没有发布。
如果你和我一样，需要用api往自己的ghost站中post文章或则获得文章列表等等，那么本文正是你需要的。

### 首先，来看下这个 API Documentation。
[[WIP] API Documentation](https://github.com/TryGhost/Ghost/wiki/%5BWIP%5D-API-Documentation?__hstc=10303082.da69975c0d2dd033ea734cd7636790a0.1419642547410.1419642547410.1419645001526.2&__hssc=10303082.3.1419645001526&__hsfp=2525482295)

**主要讲 现在实现了哪些接口**
##### Posts

    GET /ghost/api/v0.1/posts - get all posts
    Options:
    page - pagination (default: 1)
    limit - number of posts per page (all or an integer. default: 15)
    status - status of the page (all, published, draft)
    staticPages - include static pages (default: false)
    POST /ghost/api/v0.1/posts - add new post
    GET /ghost/api/v0.1/posts/:id - get post with id
    GET /ghost/api/v0.1/posts/slug/:slug - get post with slug
    PUT /ghost/api/v0.1/posts/:id - update post with id
    DELETE /ghost/api/v0.1/posts/:id - delete post with id
    
##### DB

    GET /ghost/api/v0.1/db/ - export database
    POST /ghost/api/v0.1/db/ - import database
    DELETE /ghost/api/v0.1/db/ - delete content from database
    
##### Notifications

    DELETE /ghost/api/v0.1/notifications/:id - delete notification
    POST /ghost/api/v0.1/notifications/ - add new notification
    
##### Settings

    GET /ghost/api/v0.1/settings/ - get all settings
    Options:
    type (blog, app, theme)
    GET /ghost/api/v0.1/settings/:key/ - get setting with key
    PUT /ghost/api/v0.1/settings/ - update settings
    
##### Tags

    GET /ghost/api/v0.1/tags/ - get all tags
    
##### Users

    GET /ghost/api/v0.1/users/ - get all users
    GET /ghost/api/v0.1/users/:id/ - get user with id (id=me would be the current user)
    GET /ghost/api/v0.1/users/slug/:slug/ - get user with slug
    GET /ghost/api/v0.1/users/email/:email/ - get user with email
    PUT /ghost/api/v0.1/users/:id/ - update user with id
    
###### 以post为例子
###### 对象

**Post**

    {
        posts: [
            {
                status: "published",
                id: 1,
                uuid: "ec630e45-3342-4d7f-a24c-e448263c975b",
                title: "Welcome to Ghost",
                slug: "welcome-to-ghost",
                markdown: "",
                html: "",
                image: null,
                featured: false,
                page: false,
                language: "en_US",
                meta_title: null,
                meta_description: null,
                author: 1,
                created_at: "2014-04-15T12:36:28.353Z",
                created_by: 1,
                updated_at: "2014-04-15T12:36:28.353Z",
                updated_by: 1,
                published_at: "2014-04-15T12:36:28.363Z",
                published_by: 1,
                tags: [{ ... }]
            }
        ]
    }
    
###### 参数
以发布一个post为例，posts对象必须填写。

**以下属性必须**

    title
    markdown
    
**可选属性**

    slug
    status
    image
    featured
    page
    language
    meta_title
    meta_description
    author

如果你想直接发布，那么就把status设置为published。

###### 返回
成功后的posts对象。

在你觉得post一片文章很简单的时候，你需要看看下面这个。
### 其次，调用api前，请先付钱，哦，是先oAuth
[How does oAuth work with Ghost?](https://github.com/TryGhost/Ghost/wiki/How-does-oAuth-work-with-Ghost%3F)

一句话就是你要调用api，必须先oAuth。就算是查询一个posts，也需要，因为Ghost还没有公开它的api。
怎么做呢？

###### 1、提供用户名和密码

###### 2、向Ghost Admin 发送请求以获得 access token:

	POST /ghost/api/v0.1/authentication/token
	Content-Type: application/x-www-form-urlencoded
	grant_type=password&username=<username>&password=<password>&client_id=ghost-admin
	
* grant_type: name of the authentication type
* username: email address
* password: secret password
* client_id: name of the client that is requesting access on behalf of the user
* Note: The username parameter requires your email address, rather than your username.

###### 3、Ghost Server返回  access 和 refresh token

	{
	    access_token: <access_token>
	    refresh_token: <refresh_token>
	    expires_in: 3600
	    token_type: "Bearer"
	}
* access_token: users access token; valid for 1 hour
* refresh_token: users refresh token; valid for 24 hours
* expires_in: validity of the access_token in seconds
* token_type: access token type

###### 4、开始请求资源，并附上你的access token

	GET /ghost/api/v0.1/users/me/
	Authorization: Bearer <access_token>
	
###### 5、如果access token过期怎么办？用refresh token再获得一个。

	POST /ghost/api/v0.1/authentication/token
	Content-Type: application/x-www-form-urlencoded
	grant_type=refresh_token&refresh_token=<refresh_token>&client_id=ghost-admin
	
* grant_type: name of the authentication type
* refresh_token: refresh token
* client_id: name of the client that is requesting access on behalf of the user

返回

	{
	    access_token: <access_token>
	    expires_in: 3600
	    token_type: "Bearer"
	}
	
	
	
### 一个例子：发布一个post
**请特别注意例子和官方doc中内容的区别哦。**

**请求：**

	http://api.salehao.com/ghost/api/v0.1/posts/
	Authorization: Bearer <access-token>
	Content-Type: application/json
	{"posts":[{"title":"test publishment to Ghost","status":"published","markdown":"This post will published at post cli."}]}

**返回：**

	{"posts":[{"id":3,"uuid":"738237b3-218d-4f21-85dd-c83a759b8bc0","title":"test publishment to Ghost","slug":"test-publishment-to-ghost","markdown":"This post will published at post cli.","html":"<p>This post will published at post cli.</p>","image":null,"featured":false,"page":false,"status":"published","language":"en_US","meta_title":null,"meta_description":null,"created_at":"2014-12-27T06:46:32.257Z","created_by":1,"updated_at":"2014-12-27T06:46:32.257Z","updated_by":1,"published_at":"2014-12-27T06:46:32.257Z","published_by":1,"author":1,"url":"/test-publishment-to-ghost/"}]}
	
效果：

**有了api，可以再扩展以下，用邮件发送你的文章到后台邮箱，然后自动发布？
做成插件，做成服务？**