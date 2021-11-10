![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635469849654-b5eaa3cd-1046-4d40-a43a-2c83959a747f.png#clientId=u83c9a0ec-e271-4&from=paste&height=207&id=ub8bf1879&margin=%5Bobject%20Object%5D&name=image.png&originHeight=414&originWidth=715&originalType=binary&ratio=1&size=29909&status=done&style=none&taskId=u11b42ad9-0676-4a3d-a955-9df790b2912&width=357.5)


### 5.1.1 Oauth2概念 
#### 概念
OAuth 2 是一个工业级别的开发授权协议，可以使互联网用户授权第三方网站或应用访问他们在特定网站上的信 息（如个人资料、照片等），而不必向第三方网站或应用提供密码。在此之前我们使用用户名和密码的方式登录 到应用，聪明的互联网前辈们发明了 OAuth 授权协议提出了一个 “授权服务器”，经过用户授权后授权服务器 向第三方应用发放一个访问令牌，这样就可以在不向第三方应用提供账号和密码的情况下，通过令牌在特定时间内访问用户存放在特定资源服务器上的资源 
​

#### 学习资源  
概念入门：[彻底理解 OAuth2 协议 ](https://www.bilibili.com/video/BV1zt41127hX?from=search&seid=15223742023959709802)
实战入门：[快速入门Spring Security OAuth2.0认证授权](https://www.bilibili.com/video/BV1VE411h7aL?from=search&seid=18318661601501047866)
​

## 5.1.2 OAuth2接口调用 
### 目前主要支持的oauth协议 
#### 
#### 一、密码模式 


密码模式(password)主要针对自家应用，可信度较高，所以可以使用简便安全共存的模式，操作步骤如下 ：
调用 http://localhost/bke-auth/oauth/token 传入对应的参数 
**请求头： **
```java
Tenant-Id : 000000 (替换为对应的租户id) 
Authorization ： Basic c3dvcmQ6c3dvcmRfc2VjcmV0 （"c3dvcmQ6c3dvcmRfc2VjcmV0"为 clientId:clientSecret串转换为的base64编码,需要和 tp_client 表的对应字段相匹配） 
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635845977226-864e6b50-1c7a-4cf8-9c0d-ba49013b16ae.png#clientId=u8109e883-cb0a-4&from=paste&height=267&id=ua6b9d64d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=534&originWidth=2302&originalType=binary&ratio=1&size=205656&status=done&style=none&taskId=udb7f8f92-7902-4bfc-a6f9-5fdabeb66ca&width=1151)
**表单： **
```java
grant_type ： password 
scope ： all 
username ： admin 
password ： 21232f297a57a5a743894a0e4a801fc3 
```
**注意：**

- 其中的 sword 和 sword_secret 分别是 tp_client 表 client_id 和 client_secret 字段值，请一一对应。 
- 框架对密码进行了二次加密，由前端调用传参需要现将原密码进行md5加密后再进行传递，原密码是 admin ，所以md5加密后是21232f297a57a5a743894a0e4a801fc3 ，具体如下 :

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635846040146-2eb2ce85-4d74-4cf6-be11-2fb2eb90fc96.png#clientId=u8109e883-cb0a-4&from=paste&height=329&id=u5c638004&margin=%5Bobject%20Object%5D&name=image.png&originHeight=658&originWidth=2304&originalType=binary&ratio=1&size=296809&status=done&style=none&taskId=u73e3a8e8-88f8-4980-ab91-05541d79e29&width=1152)


**调用认证接口返回结果 :**
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635846466030-6c3e1025-7597-4590-ad50-f7b1582187e6.png#clientId=u8109e883-cb0a-4&from=paste&height=418&id=u53235eca&margin=%5Bobject%20Object%5D&name=image.png&originHeight=836&originWidth=2700&originalType=binary&ratio=1&size=371283&status=done&style=none&taskId=ue4a3730b-795f-4927-b859-0ff64443715&width=1350)


![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635846535468-c9d404f4-1bef-4580-90a5-bb7e5a55798a.png#clientId=u8109e883-cb0a-4&from=paste&height=823&id=u67871c97&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1646&originWidth=2874&originalType=binary&ratio=1&size=1763259&status=done&style=none&taskId=u8309ba33-29b5-4b0d-93c1-6de395b5912&width=1437)
#### 
#### 二、刷新token 


刷新token(refresh_token)存在时间会比access_token更长，主要用于access_token快过期的时候，调用oauth 接口获取到刷新后的token以达到token续期的目的
调用 http://localhost/bke-auth/oauth/token 传入对应的参数 
**请求头： **
```json
Tenant-Id : 000000 (替换为对应的租户id) 
Authorization ： Basic c3dvcmQ6c3dvcmRfc2VjcmV0 （"c3dvcmQ6c3dvcmRfc2VjcmV0"为 clientId:clientSecret串转换为的base64编码,需要和 tp_client 表的对应字段相匹配） 
```


**表单： **
```json
grant_type ： refresh_token 
scope ： all 
refresh_token : 
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0ZXN0IjoidGVzdCIsInVzZXJfbmFtZSI6ImFkbWluIiwic2NvcGUiOlsiYWxsIl0sImV4cCI6MTU1MzE2MTA5NSwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6IjE0YmMyYjAyLTgxY2UtNDFiNC04ZTI3LTA5YWE0ZmU4ZWMwYyIsImNsaWVudF9pZCI6ImJsYWRlIn0.jTmioQDq-fSNNn7YCwl3wP0JE-etSWtzLDe545mDbP4 
```


#### 三、 授权码模式 


授权码模式(authorization_code)主要针对第三方应用，是最为复杂也最为安全的一种模式，操作步骤如下: 
​


1. 打开浏览器访问如下地址：

http://localhost:8100/oauth/authorize?tenant_id=000000&client_id=sword&redirect_uri=http://localhost:8888&code=233333&response_type=code 

2. 输入用户名为 admin ，密码为md5(admin)也就是上文提到的 21232f297a57a5a743894a0e4a801fc3 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635847133028-e0bfd507-1384-4b9b-a912-a5cad1299b14.png#clientId=u8109e883-cb0a-4&from=paste&height=263&id=O9xDo&margin=%5Bobject%20Object%5D&name=image.png&originHeight=526&originWidth=2302&originalType=binary&ratio=1&size=301241&status=done&style=none&taskId=u74b1d925-8901-484e-a087-fd35c609759&width=1151)

3. 点击Authorize按钮，通过授权 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635847599782-6d1d0338-6bec-49a9-b50f-722eda5c864f.png#clientId=u8109e883-cb0a-4&from=paste&height=314&id=u10753e10&margin=%5Bobject%20Object%5D&name=image.png&originHeight=628&originWidth=2294&originalType=binary&ratio=1&size=297646&status=done&style=none&taskId=u2de48a62-a007-497a-947d-e5e41df68b0&width=1147)

4. 系统自动跳转至http://localhost:8888并加上了code参数 
4. 获取跳转后的code值(http://localhost:8888/?code=VhYNLR)之后，调用 http://localhost/bke-auth/oauth/token 传入对应的参数



**请求头：** 
```json
Tenant-Id : 000000 (替换为对应的租户id) 
Authorization ： Basic c3dvcmQ6c3dvcmRfc2VjcmV0 （"c3dvcmQ6c3dvcmRfc2VjcmV0"为 
clientId:clientSecret串转换为的base64编码,需要和 blade_client 表的对应字段相匹配） 
```
**表单： **
```json
grant_type ： authorization_code 
scope ： all 
code ： VhYNLR 
redirect_uri ： http://localhost:8888 
```


**注意：** 

- 其中的 sword 和 sword_secret 分别是 tp_client 表 client_id 和 client_secret 字段值，请一一对应。 
- 其中的 http://localhost:8888 是 tp_client 表的 web_server_redirect_uri 字段 值，也请一一对应。 

​

#### 四、获取到token后如何获取用户信息 
​

**1. 拼接请求头 **
```json
Authorization ：bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0ZXN0IjoidGVzdCIsInVzZXJfbmFtZSI6ImFkbWluIiwic2NvcGUiOlsiYWxsIl0sImV4cCI6MTU1MzE2MTA5NSwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6IjE0YmMyYjAyLTgxY2UtNDFiNC04ZTI3LTA5YWE0ZmU4ZWMwYyIsImNsaWVudF9pZCI6ImJsYWRlIn0.jTmioQDq-fSNNn7YCwl3wP0JE-etSWtzLDe545mDbP4 
```
**2. 调用 http://localhost/bke-auth/oauth/user-info 既可获得对应用户信息 **
## 5.1.3 Swagger配置
#### 配置客户端 

- 打开接口文档系统http://localhost:18000/doc.html，设置客户端认证的请求头。（"c3dvcmQ6c3dvcmRfc2VjcmV0"为clientId:clientSecret串转换为的base64编码 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635845977226-864e6b50-1c7a-4cf8-9c0d-ba49013b16ae.png#clientId=u8109e883-cb0a-4&from=paste&height=267&id=TDZdm&margin=%5Bobject%20Object%5D&name=image.png&originHeight=534&originWidth=2302&originalType=binary&ratio=1&size=205656&status=done&style=none&taskId=udb7f8f92-7902-4bfc-a6f9-5fdabeb66ca&width=1151)

- 点击左上角的 Authorize ，配置 Authorization 请求头 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635849710846-6526af52-be12-46ee-bb19-5dab7d3cc157.png#clientId=u8109e883-cb0a-4&from=paste&height=610&id=ufeed2989&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1220&originWidth=3356&originalType=binary&ratio=1&size=608247&status=done&style=none&taskId=u348f1369-141f-42d1-8382-3c6ab74dbff&width=1678)
#### 配置token 

- 获取上一章节中调用 /oauth/token 接口返回的参数 token_type 和 access_token ，将他们拼接起来并以逗号隔开 
- 点击左上角的 Authorize ，配置 Techpower-Auth 请求头 
- 请求头对应的值为 tokenType + ' ' + accessToken 
- 配置租户编号，这里我们设置为 000000 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635849997759-79a77c77-6387-4411-813a-5e87656d72f7.png#clientId=u8109e883-cb0a-4&from=paste&height=423&id=uf0d14d1e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=846&originWidth=3358&originalType=binary&ratio=1&size=429868&status=done&style=none&taskId=ua32cab37-f529-4b20-ab3b-3da43c90d2d&width=1679)
#### 配置生效 
保存完毕后浏览器刷新，再打开对应的接口，这时候会发现接口直接调用成功 
​

## 5.1.4 接口鉴权配置 
### 接口调用说明 
上一章节的swagger是如此配置，其实后续用postman调用api接口的时候也是如此 
### Authorization请求头获取
设置客户端认证的请求头，设置请求头为 Authorization ，请求头对应的值为 
Basic c3dvcmQ6c3dvcmRfc2VjcmV0 （"c3dvcmQ6c3dvcmRfc2VjcmV0"为clientId:clientSecret串转换为的base64编码 ）
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635845977226-864e6b50-1c7a-4cf8-9c0d-ba49013b16ae.png#clientId=u8109e883-cb0a-4&from=paste&height=267&id=q9YR7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=534&originWidth=2302&originalType=binary&ratio=1&size=205656&status=done&style=none&taskId=udb7f8f92-7902-4bfc-a6f9-5fdabeb66ca&width=1151)
### Techpower-Auth请求头获取 
获取上一章节中调用 /oauth/token 接口返回的参数 token_type 和 access_token ，将他们 拼接起来并以逗号隔开配置 Techpower-Auth 请求头，请求头对应的值为 tokenType + ' ' + accessToken 
​

## 5.1.5 接口放行配置 

- 基于Nacos的动态网关鉴权功能 
- 鉴权放在了gateway,鉴权配置放在nacos，并且支持动态刷新 
- 注： bke-demo 这一类前缀，是网关转发的key，不是controller的地址，所以不需要加上。
- 配置写法如下： 
```json
techpower: 
secure: 
skip-url: 
- /test/** 
- /demo/** 
```


- 网关鉴权逻辑代码位置如下： 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635849142364-6865fa91-506e-466c-8c33-2cc677b52483.png#clientId=u8109e883-cb0a-4&from=paste&height=425&id=uff48263d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=850&originWidth=822&originalType=binary&ratio=1&size=278530&status=done&style=none&taskId=uf6475523-a2bf-4b2c-8e47-bfa0288029a&width=411)
​

## 5.1.6 新应用授权 

1. 系统管理->应用管理模块 
1. 新增应用

主要输入：客户端id、客户端密钥、授权范围、授权类型、回调地址、token过期秒数、 refresh_token过期秒数 

3. 自家应用可采用密码模式进行授权校验（password） 

调用 http://localhost/blade-auth/oauth/token 传入对应的参数进行测试 
设置请求头： 
Authorization ： Basic c3dvcmQ6c3dvcmRfc2VjcmV0 （"c3dvcmQ6c3dvcmRfc2VjcmV0"为 
clientId:clientSecret串转换为的base64编码） 
表单参数： 
grant_type ： password 
scope ： all 
username ： admin 
password ： 21232f297a57a5a743894a0e4a801fc3 

4. 进行应用设置 

授权应用配置 clientId、 clientSecret 的值 

5. 启动前端与后端服务， 登录测试是否可以认证成功 



## 5.1.7 第三方系统登录 
### 5.1.7.1 概念说明 
#### 什么是第三方登录 
第三方登录，是基于用户在第三方平台上已有的账号和密码来快速完成己方应用的登录或者注册功能。而这里的 第三方平台，一般是已经拥有大量用户的平台，国外的比如Facebook，Twitter等，国内的比如微博、微信、QQ 
等。
#### 为什么需要第三方登录 
**对于用户来说**：采用第三方登录的应用，用户只需要首次授权，之后都可以直接登录，无需记忆多个系统的 密码，只需记住一个即可。比如采用最常用的微信登录，打开应用的时候只需微信扫一扫便可直接登录成功，连密码都无需记忆，非常方便。 
**对于应用来说**：降低了用户的注册成本，提高了用户的转化率。通过授权，可以通过用户允许，获取其比如 好友、粉丝等基础信息，可以通过这些授权获得的数据进行合理的运营推广，增加自身的知名度，转化更多的定向用户。 
**对于第三方来说**：增加了用户对自身平台的依赖，用于使用次数越多，则越能增加用户粘性。只要越来越多 
的应用集成自身平台，便会被更广大的用户所熟知，有利于为平台增加更多的用户。 
**总结**：不论对于用户还是应用还是第三方，这种方案都具有较大的优势，所以越来越多的应用会选择集成第 
三方系统登录功能。 
#### 采用哪种技术实现 
目前已有较为成熟的解决方案，名为JustAuth，BladeX就是基于他来实现了便捷的第三方登录，JustAuth开源地址：https://github.com/justauth/JustAuth 史上最全的整合第三方登录的开源库。目前已支持Github、Gitee、微博、钉钉、百度、Coding、腾讯云开 发者平台、OSChina、支付宝、QQ、微信、淘宝、Google、Facebook、抖音、英、小米、微软、今日头 条、Teambition、StackOverflow、Pinterest、人人、华为、企业微信、酷家乐、Gitlab、美团、饿了么和推特等第三方平台的授权登录。 
Login, so easy! 
### 5.1.7.2 对接说明 


1. 为了让大家更轻松地接入系统，请务必先浏览一遍JustAuth的文档，有个大概了解后会降低对接的难度，文档地址：https://justauth.wiki/#/?id=%e7%ae%80%e4%bb%8b 
2. 我们采用最简单的Gitee第三方登录为例。其他第三方获取key的具体流程，请参考JustAuth的文档，最终获 
取到key之后将其配置到BladeX系统中便可以直接生效 
### 5.1.7.3 对接准备
#### 创建应用并获取Key 
1. 登录Gitee : https://gitee.com 
2. 进入第三方应用：https://gitee.com/oauth/applications 
3. 创建应用，填上所需的配置，重点为：应用回调地址 

- 应用名称：填写自己的网站名称 
- 应用描述： 填写自己的应用描述 
- 应用主页： 填写自己的网站首页地址 
- 应用回调地址： 【重点】该地址为用户授权后需要跳转到的自己网站的地址 

1）这里填写为 ( http://127.0.0.1:1888/oauth/redirect/gitee ) 
2）其中分为两部分，一部分为前缀：( http://127.0.0.1:1888 )，一部分为后缀( gitee )，中间的( 
/oauth/redirect )为固定 
3）前缀为最终部署的前端地址，因为授权成功后会直接跳转到对应的前端，然后携带所需的参数，前端再进 
行解析并登录，所以这个地址不能出错 
注意：若部署在nginx，需要给这个回调地址加一段配置，否则会触发404，具体配置如下 
```xml
location ^~ /oauth/redirect { 
rewrite ^(.*)$ /index.html break; 
} 
```


4）后缀需要与JustAuth内定义的Key相对应，一般填入Key小写。具体Key的枚举，请看： 
https://github.com/justauth/JustAuth/blob/master/src/main/java/me/zhyd/oauth/config/AuthDefa 
ultSource.java 
5） 举个例子，若我们需要配置的是微博的第三方登录，那么回调地址就是 (http://127.0.0.1:1888/oauth/redirect/weibo ) 
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635855623152-4d3c882d-c242-4022-8b3e-a5e88330b0b3.png#clientId=u8109e883-cb0a-4&from=paste&height=771&id=u35072c51&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1542&originWidth=1372&originalType=binary&ratio=1&size=1524923&status=done&style=none&taskId=u0c557ec6-3151-4d01-8e2d-5caa0742402&width=686)

- 权限： 根据页面提示操作，默认勾选第一个就行 

4. 以上信息输入完成后，点击确定按钮创建应用。创建完成后，点击进入应用详情页，可以看到应用的密钥等信息 
5. 等到我们获取到clientId与clientSecret后，准备工作就结束了，接下来我们把他配置到BladeX中，看一下第三方登录是否生效 
### 5.1.7.4 配置说明 
#### 将应用的Key配置到系统 

1. 配置文件在 bke-auth 的 application.yml 与 application-xx.yml 内  
1. 配置上一节获取到的密钥，注意 redirect-url 需要与刚刚在应用配置的应用回调地址一致 

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635855881426-ca82eb9e-8c30-46f9-b500-325fa247ae69.png#clientId=u8109e883-cb0a-4&from=paste&height=830&id=u9dda9cd7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1660&originWidth=2334&originalType=binary&ratio=1&size=1914313&status=done&style=none&taskId=u25652eba-4754-4d9c-8dc2-0171a30277c&width=1167)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635855924249-b34a39ae-5ac6-4521-b679-a0658c0ef5df.png#clientId=u8109e883-cb0a-4&from=paste&height=364&id=ud2ef5c2c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=728&originWidth=1712&originalType=binary&ratio=1&size=666900&status=done&style=none&taskId=u6a845bd7-8fe4-4476-8b97-b5bb511d0af&width=856)

3. 前端配置第三方授权地址，对应的是后端开放的API

​

### 5.1.7.5 操作流程 
#### 第三方系统登录详细操作流程 
1. 启动前后端，访问前端首页 
2. 点击跳转，可以看到，地址栏的clientId与我们刚刚配置的一致 
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635856229698-1f763bf0-ab73-4f1f-9e95-84d5754e6358.png#clientId=u8109e883-cb0a-4&from=paste&height=398&id=ue6f2b20e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=796&originWidth=1358&originalType=binary&ratio=1&size=408907&status=done&style=none&taskId=u5bdc0040-dbf3-429f-a8a8-2c143892bb8&width=679)
3. 输入账号密码点击登录，同样可以看到，地址栏的信息与我们配置的一致 
4. 点击同意授权，便会自动跳转会我们的前端系统，然后进行登录 
5. 登录成功后系统会在后台检测该用户是否已经注册在系统内拥有自己的账号，若没有，第一次登录成功会 
弹出一个小框提供快速注册。（注：目前很多网站，使用第三方登录后，都会提示再注册一个账号或者使用 
手机+验证码的方式快速注册，否则只可以使用基础功能。这是常用的一个套路，因为这么做了就可以获取 
到用户的信息，提升用户粘性，若是手机注册，后续还可以发送营销短信，从商家的角度考虑，这无形中是 
一个非常省钱收益又大的功能。从用户角度考虑，只要营销不是太过分，都会默认接受。）（如果采用租户 
+定制域名的方式部署，则系统会自动读取租户编号，快捷注册的时候不会再弹出。只有使用通用域名没有 
绑定租户的时候，才会弹出租户编号让用户自行选择注册再那个租户下，具体租户域名配置，请看文档5.2.2 
章节） 
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635856605272-f9d78386-c3b1-4edb-bfb3-11fe0ab76456.png#clientId=u8109e883-cb0a-4&from=paste&height=828&id=ue95ce1cf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1656&originWidth=1290&originalType=binary&ratio=1&size=328967&status=done&style=none&taskId=u5113000e-16eb-40dc-9dce-b53fa921638&width=645)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635856623070-3871aa1e-2c99-45fb-a365-349e88b6a96d.png#clientId=u8109e883-cb0a-4&from=paste&height=197&id=eM5ln&margin=%5Bobject%20Object%5D&name=image.png&originHeight=394&originWidth=1266&originalType=binary&ratio=1&size=110306&status=done&style=none&taskId=ub3107d8d-cc88-488d-8dba-ce15d20a64c&width=633)
6. 我们输入基本信息和账号密码，并点击提交，最后就会跳转回系统首页 
7. 这时大家会发现，注册成功后，再次点击gitee登录，会重新回到首页并无法登录。因为我们现在的定位是后台管理系统，安全要求更高一些，必须要有指定的角色才可以登录。若大家不需要这么严格，可以自行简单修改逻辑，给第三方注册的用户默认分配一个guset角色，这样就可以登录查看最基础的菜单了。 
8. 下面我们登录租户管理员，给刚注册的用户分配角色、部门、岗位，然后再进行登录测试 
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1141782/1635856672742-2388d355-deb3-4feb-b25a-a86015bc603d.png#clientId=u8109e883-cb0a-4&from=paste&height=428&id=TeN7S&margin=%5Bobject%20Object%5D&name=image.png&originHeight=856&originWidth=1366&originalType=binary&ratio=1&size=381953&status=done&style=none&taskId=u0f1ee596-9a32-4e22-97fa-53c43108f49&width=683)
9. 提交后再次访问gitee三方登录，可以看到我们登录成功，菜单权限也与分配的角色一致 
10. 因为我们刚刚快速注册了账号，那么除了第三方登录外，后续也可以直接使用账号来登录了，非常方便 
11. 授权成功，我们会收到一份官方发过来的邮件，提示哪些应用被授权了 
