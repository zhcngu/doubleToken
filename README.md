# doubleToken
asp.netCore web api double token ---accessToken and refreshToken
本文基于 asp.net Core6 版本简单，简单阐述了 双token机制。即AccessToken 和 RefreshToken.
web端登录时，服务器返回 两个token 即{ code=200,msg="success",data={accessToken="123456",refreshToken:"987654"}}
web端一般存储在 local Storage中。
然后再发起ajax 请求时，会在请求头中 加入accessToken.
服务端会检查传入的accessToken 如果正确且未过期，则返回数据。如果过期则服务器端可以返回一个特殊的过期标志。该标志目的让前端知道这个token已经过期，应该刷新token了。如果token错误，一般直接返回401状态码，通知前端重新登录。
前端根据服务端返回的消息，判断是要刷新token 还是重新登录。如果是刷新token 将accessToken和refreshToken 发送到服务端。
服务端接受 accessToken和refreshToken。从accessToken中接续出来用户的身份信息。解析refreshToken的过期时间，如果没过期，则重新创建一个accessToken，并且将新的accessToken和refreshToken 在返回给前端。如果refreshToken 过期了。则服务器直接返回401.通知前端要重新登录
前端服务器返回的数据，如果是401，则直接跳转到登录页，如果刷新token成功，并且得到新的accessToken和refreshToken。替换 local Storage中的两个token,然后重新发起请求，获取数据。实现无感的token刷新。
