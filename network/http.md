## `get`与`post`的区别
1. POST和GET都是HTTP请求的基本方法。
2. 区别主要有以下几个：
  - GET 请求在浏览器刷新或者回退的时候是无害的。POST的话数据会被重新提交。
  - GET可以被书签收藏，POST不行
  - GET可以存在缓存中。POST不行
  - GET 会将数据存在浏览器的历史中，POST不会
  - GET 编码格式只能用ASCII码，POST没有限制
  - GET 数据类型urlencode,POST是URLENCODE，form-data
  - 可见性 参数在URL用户可以看见，POST的参数在REQUSET BODY中不会被用户看见
  - 安全性 GET相对不安全 POST相对安全些
  - 长度 参数一般限制2048（和WEB服务器相关），参数无限制。
3. GET 和POST在请求的时候
  - GET 是将数据中的hearder 和 data 一起发送给服务端，返回200code
  - POST 是先将hearder发给服务器返回100continue，再发送data给到服务器，返回200
  - GET 就发送了一个TCP数据包给服务器而POST发送了两次TCP数据包给服务器
  - GET和POST是已经有定义好的说明的，最好不要混用。
4. GET和POST本质上是一样一样的，GET可以加Request Body ，POST也可以在URL中添加参数。实现是可以的。


## 常见状态码
- 1XX：服务器接收到请求
- 2XX：成功
- 3XX：重定向
- 4XX：客户端错误
- 5XX：服务端错误


- 200: 请求成功
- 301：永久重定向
- 302：临时重定向
- 401：未授权，服务器拒绝请求
- 403：禁止，服务器禁止请求
- 404：未找到
- 502：错误网关