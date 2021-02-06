# wiki.upare.com
[TOC]
## 1: System

###### Error codes

| codes (int) | Description (string)         | 错误描述 (文本)  |
| :---------: | :--------------------------: | :--------------: |
| 100001      | Internal error               | 内部错误（系统） |
| 100002      | POST error                   | 提交错误         |
| 100003      | Datebase error               | 数据库错误       |
| 100004      | No data                      | 无数据           |
| 100005      | Failed to pass the validator | 验证器验证失败   |
| 100006      | Failed to update data        | 更新数据失败     |
| 100007      | Failed to delete data        | 插入数据失败     |
| 100008      | Failed to insert data        | 插入数据失败     |
| 100009      | Failed to query data         | 查询数据失败     |
| 100010      | Unknown error                | 未知错误         |
| 100011      | Invalid method               | 方法错误         |

## 2: Services 服务

### 200: Common 通用

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :--------------: |
| 200      | Success              | 成功             |
| 200001      | Invalid parameter|参数错误（服务）|
| 200002      | Submission method should be POST|提交方式应为POST|
| 200003      | Non Ajax request|非AJAX请求|
| 200004      | Invalid captcha|验证码错误|
| 200005      | Captcha timeout|验证码超时|
| 200006      | Captcha is required|验证码不能为空|
| 200007      | Session has expired|会话过期|
| 200008      | Request exceeded limit|请求超限|
| 200009      | Invalid token|非法的数据令牌|
| 200010      | Token is required|数据令牌不可为空|
| 200011      | Permission denied|没有权限|
| 200012      | Username is required|用户名不能为空|
| 200013      | Username should be more than 2 characters|用户名不得小于2个字符|
| 200014      | Username should be less than 50 characters|用户名不得大于50个字符|
| 200015      | Username can only be Chinese characters, letters, numbers, underscores and dashes |用户名只能由汉字、字母、数字、下划线及破折号组成|
| 200016      | Invalid username|用户名无效|
| 200017      | Password is required|密码不能为空|
| 200018      | Password at least 6 characters|密码至少6个字符|
| 200019      | Password inconsistent|密码不一致|
| 200020      | Email is required|邮箱地址不可为空|
| 200021      | Invalid email|邮箱格式不正确|
| 200022      | Phone number is required|电话号码不能为空|
| 200023      | Invalid phone number|电话号码格式错误|
| 200024      | Phone number must be 11 digits|电话号码必须是11位|
| 200025      | The phone number is not supported|不支持该号段|
| 200026      | Invalid ip|IP地址错误|

### 201: Oauth2.0

#### 2011:authorize

OAuth2.0的authorize接口

###### Usage

**url:** /apis/Oauth/authorize

**method:** GET/POST

| Parameter     | Type   | REQUIRED | Remarks                                                      |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| response_type | string | true     | json OR code                                                 |
| client_id     | string | true     | 申请应用时分配的AppKey                                       |
| redirect_uri  | string | true     | 授权回调地址，站外应用需与设置的回调地址一致，站内应用需填写canvas page的地址 |
| scope         | string | false    | 申请scope权限所需参数，可一次申请多个scope权限，用逗号分隔   |
| state         | string | false    | 用于保持请求和回调的状态，在回调时，会在Query Parameter中回传该参数。开发者可以用这个参数验证请求有效性，也可以记录用户请求授权页前的位置。这个参数可用于防止跨站请求伪造（CSRF）攻击 |

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :--------------: |
| 201101      | Authorization code cannot be empty |授权码不能为空|
| 201102      | Invalid authorization code |授权码验证失败|
| 201103      | Invalid client_id | client_id 无效 |
| 201104      | redirect_uri and client_id are not matched  or client not activated | redirect_uri和client_id不匹配或客户端未激活 |
| 201105      | redirect_uri and client_id are required | redirect_uri和client_id不能为空 |
| 201106      | Invalid response_type | response_type参数无效 |

###### Example

> 
> 请求
> https://www.upare.com/apis/Oauth/authorize/?client_id=123050457758183&redirect_uri=http://www.example.com/response&response_type=code
> 
> 同意授权后会重定向
> http://www.example.com/response&code=CODE

###### Return

```json
{"code":200,"request":"apis/Oauth/authorize","authorization_code":"a7cbae5f75ea7514ed963182d8ae43f71dcb1e88","message":"Successful"}
```

> **authorization_code** (string): 用于提交到access_token接口的code

#### 2012: Access token

OAuth2.0的access_token接口

###### Usage

**url:** /apis/Oauth/accessToken

**method:** POST

| Parameter     | Type   | REQUIRED | Remarks                                                      |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| client_id     | string | true     | 申请应用时分配的AppKey                                       |
| client_secret | string | true     | 申请应用时分配的AppSecret                                    |
| grant_type    | string | true     | 请求的类型，填写authorization_code/refresh_token/password    |
| redirect_uri  | string | true     | 回调地址，需需与注册应用里的回调地址一致                     |
| code          | string | false    | grant_type为authorization_code时，调用/apis/Oauth/authorize获得的code值 |
| refresh_token | string | false    | grant_type为refresh_token时，通过授权得到的refhresh_token刷新access_token |
| username      | string | false    | 系统用户名                                                   |
| password      | string | false    | 系统密码                                                     |

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :--------------: |
| 201201      | Invalid access token |非法token |
| 201202      | Access token is required |数据令牌不可为空 |
| 201203      | Access token expires or does not exist |数据令牌过期或不存在 |
| 201204      | API requests out of rate limit |接口请求超限 |
| 201205      | client_id client_secret grant_type redirect_uri are not matched |client_id client_secret grant_type redirect_uri不匹配 |
| 201206      | Api permission denied |无权访问该接口 |
| 201207      | Delete token failed |删除token失败 |
| 201208      | client_id client_secret are not matched |client_id client_secret不匹配 |
| 201209      | Invalid refresh_token |refresh_token无效 |
| 201210      | client_id is required |client_id不能为空 |
| 201211      | client_secret is required |client_secret不能为空 |
| 201212      | Access username and password are required |username和password不能为空 |
| 201213      | client_id not exist |client_id不存在 |
| 201214      | client_id and username are not matched |client_id和username不匹配 |

###### Return

 ```json
 {"code":200,"request":"apis/Oauth/accessToken","access_token": "ACCESS_TOKEN","refresh_token": "REFRESH_TOKEN","expires_in": 1234,"remind_in":3600,"uid":12341234}
 ```

>  **access_token** (string): 用户授权的唯一票据，用于调用开放接口，同时也是第三方应用验证用户登录的唯一票据，第三方应用应该用该票据和自己应用内的用户建立唯一影射关系，来识别登录状态，不能使用本返回值里的UID字段来做登录识别。
>
>  **refresh_token** (string): grant_type为refresh_token时可通过refresh_token获得access_token。
>
>  **expires_in** (int): access_token的生命周期，单位是秒数。

#### 2013: Token Information

查询用户access_token的授权相关信息，包括授权时间，过期时间和scope权限。

###### Usage

**url:** /apis/Oauth/info

method: POST

| Parameter    | Type   | REQUIRED | Remarks                                                      |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| access_token | string | true     | 用户授权时生成的access_token |

###### Return

```json
{"code":200,"request":"apis/Oauth/info","client_id":"client_id","uid":000000,"create_at":1580651779,"expire_in":2313,"message":"Successful"}
```

> **uid**	(string)	:	授权用户的uid
> **appkey**	(string)	:	access_token所属的应用appkey
> **scope**	(string)	:	用户授权的scope权限
> **create_at**	(int)	:	access_token的创建时间，从1970年到创建时间的秒数
> **expire_in**	(int)	:	access_token的剩余时间，单位是秒数

#### 2014: Apis remaining

获取当前登录用户的API访问频率限制情况

###### Usage

**url:** /apis/Oauth/limit

**method:** GET/POST

| Parameter    | Type   | REQUIRED | Remarks                      |
| ------------ | ------ | -------- | ---------------------------- |
| access_token | string | true     | 用户授权时生成的access_token |

###### Return

```json
{"code":200,"request":"apis/Oauth/limit","api_rate_limits":[{"api":"apis/Services/contact","limit":30,"limit_time_unit":"HOURS","remaining_hits":30},{"api":"upare/Authlogin/index","limit":30,"limit_time_unit":"HOURS","remaining_hits":30}],"message":"Successful"}
```

> **api_rate_limits**	(array)	:	应用url及其剩余时间和访问次数

#### 2015: Access_Token revoke

重置Access_Token

###### Usage

**url:** /apis/Oauth/revoke

**method:** GET/POST

| Parameter    | Type   | REQUIRED | Remarks                      |
| ------------ | ------ | -------- | ---------------------------- |
| access_token | string | true     | 用户授权时生成的access_token |

###### Return

```json
{"code":200,"request":"apis/Oauth/revoke","access_token":"4dd08419063aeb2bccdcfc00f65747effe88ad50","refresh_token":"bfd278596f53db1cee195262b46e104e374d404c","message":"Successful"}
```

#### 2016: Access_token delete

删除已经过期的Access_token

###### Usage

**url:** /apis/Oauth/delete

**method:** POST

| Parameter    | Type   | REQUIRED | Remarks                      |
| ------------ | ------ | -------- | ---------------------------- |
| access_token | string | true     | 用户授权时生成的access_token |

Return

```json
{"code":200,"request":"apis/Oauth/delete","message":"Successful"}
```



### 202: Service apis

#### 2021: guestbook

留言板接口

###### Usage

**url:** /apis/Services/guestbook

**method:** POST

| Parameter | Type   | REQUIRED | Remarks         |
| --------- | ------ | -------- | --------------- |
| name      | string | true     | Guest's name    |
| email     | string | true     | Guest's email   |
| phone     | int    | true     | Guest's phone   |
| message   | string | true     | Guest's message |
| subject   | string | false    | subject         |

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :--------------: |
| 202101 | Message delivery failed | 消息发送失败 |
| 202102 | Messages content cannot be empty | 消息不能为空 |

Return

```json
{"code":200,"request":"apis/Services/guestbook","message":"Successful"}
```

#### 2022: location

Ipv4 database and get visitors' location

###### Usage

**url:** /apis/Services/location

**method:** GET/POST

| Parameter    | Type   | REQUIRED | Remarks                                                 |
| ------------ | ------ | -------- | ------------------------------------------------------- |
| access_token | string | true     | 用户授权时生成的access_token                            |
| ip           | string | false    | Optional, if ip is null, get current visitor's location |
| format       | string | false    | json or jsonp                                           |

###### Return

/apis/services/location

```json
{"code":200,"request":"apis/Services/location","ip":"8.8.8.8","country":"美国","province":"","city":"","county":"","isp":"","area":"美国加利福尼亚州圣克拉拉县山景市谷歌公司DNS服务器","message":"Successful"}
```

/apis/services/location?ip=8.8.8.8&format=jsonp

```json
jsonpReturn({"code":"200","request":"apis/Services/location","ip":"8.8.8.8","country":"美国","province":"","city":"","county":"","isp":"","area":"美国加利福尼亚州圣克拉拉县山景市谷歌公司DNS服务器","message":"成功"});
```

### 203: Users

#### 2030: User common

Show user common functions

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :------------: |
| 203001 | The username already registered | 用户已存在 |
| 203002 | The email already exists |该邮箱已经被使用 |
| 203003 | Email and username do not match |邮箱和用户名不匹配 |
| 203004 | Password mismatch |用户名与密码不匹配 |
| 203005 | Account does not exist |账户不存在 |
| 203006 | The new password cannot be the same as the old |新密码不可与旧密码相同 |
| 203007 | Invalid identity card | 身份证号码错误 |



#### 2031: User register

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :------------: |
| 203101 | Invalid verification code | 验证码无效 |
| 203102 | Failed to send registration verification code | 注册验证码发送失败 |

#### 2032: User login

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :------------: |
| 203201 | Username does not exist | 用户名不存在 |
| 203202 | Failed login greater than %d times, account locked for %d minutes | 登录失败累计大于%d次，账户锁定%d分钟 |
| 203203 | Login failed with %s | %s登录失败 |
| 203204 | Account has been bound to %s | 该账号已经绑定%s |
| 203205 | The account of %s has not been bound to the account of this website | %s账号尚未绑定本站 |

#### 2033:User forget password

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :------------: |
| 203301 | Account required |账户不能为空 |
| 203302 | Account at least 2 characters |账户至少2个字符 |
| 203303 | Failed to reset password |重置密码失败 |

#### 2034: User common

Show user's detail information

###### Usage

**url:** /apis/Users/show

**method:** POST

| Parameter    | Type   | REQUIRED | Remarks          |
| ------------ | ------ | -------- | ---------------- |
| access_token | string | true     | access_token     |
| uid          | int    | true     | User's unique id |

###### Return

```json
{"code":200,"request":"apis/Users/show","username":"username","nickname":"demo","openid":"2da653ew18828e6405b65438545","avatar":"https://storage.upare.com/uploads/demo.jpg?temp_url_sig=f27de494d94f56f4effc3c45952924fb54163f2c&temp_url_expires=1612605017","gender":"Male","message":"Successful"}
```



### 204:Pictures

Picture encryption and decryption

#### 2041: Image encryption

###### Usage

**url:** /apis/Images/disorder

**method:** POST

| Parameter    | Type   | REQUIRED | Remarks                                  |
| ------------ | ------ | -------- | ---------------------------------------- |
| access_token | string | true     | access_token                             |
| image        | string | true     | Base64 strings, image type is jpg or png |
| password     | string | false    | password                                 |
| format       | string | false    | Return json or jsonp data                |

###### Error codes

| codes (int) | Description (string) | 错误描述 (文本)  |
| :---------: | :------------------: | :------------: |
| 204101 | Images is required |图像不能为空 |
| 204102 | Invalid image extension |非法图像扩展名 |
| 204103 | Invalid image type |非法图像类型 |

Return

```json
{"code":200,"request":"apis/Images/disorder","password":151105,"image":"data:image/png;base64....","message":"Successful"}
```

#### 2042: Image decryption

###### Usage

**url:** /apis/Images/reorder

**method:** POST

| Parameter    | Type   | REQUIRED | Remarks                                  |
| ------------ | ------ | -------- | ---------------------------------------- |
| access_token | string | true     | access_token                             |
| image        | string | true     | Base64 strings, image type is jpg or png |
| password     | string | true     | password                                 |
| format       | string | false    | Return json or jsonp data                |

###### Error codes

| codes (int) |  Description (string)   | 错误描述 (文本) |
| :---------: | :---------------------: | :-------------: |
|   204201    |   Images is required    |  图像不能为空   |
|   204202    | Invalid image extension | 非法图像扩展名  |
|   204203    |   Invalid image type    |  非法图像类型   |
|   204203    |     Password error      |    密码错误     |

###### Return

```json
{"code":200,"request":"apis/Images/reorder","image":"data:image/png;base64....","message":"Successful"}
```

