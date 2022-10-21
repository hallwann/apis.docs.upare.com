# apis.docs.upare.com
[TOC]
## 1: System

### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 0 | Successful | system.success  | 成功 |
| 200 | OK | system.success  | 成功 |
| 403 | Unauthorized | system.exception  | 未授权 |
| 404 | Not found | system.exception  | 无法找到资源 |
| 100001 | Internal error | system.common  | 内部错误 |
| 100002 | Unknown error | system.common  | 未知错误 |
| 100003 | Database error | system.common  | 数据库错误 |
| 100004 | Validation failed | system.common  | 验证失败 |
| 100005 | Mail sending failed | system.common  | 邮件发送失败 |
| 100006 | Invalid parameter | system.common  | 无效参数 |


## 2: API

### 2.0 API Common

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 200001 | POST method required | api.common  | POST方法提交 |
| 200002 | Invalid captcha | api.common  | 无效验证码 |

### 2.1 Oauth

#### 2.1.1 authorize

OAuth2.0 authorize api

###### Usage: 获取authorization code

**url:** /api/Oauth/authorize

**method:** GET

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| response_type | string | true | json OR code |
| client_id | string | true | 申请应用时分配的AppKey |
| redirect_uri  | string | true | 授权回调地址，站外应用需与设置的回调地址一致，站内应用需填写canvas page的地址 |
| scope | string | false | 申请scope权限所需参数，可一次申请多个scope权限，用逗号分隔 |
| state | string | false | 用于保持请求和回调的状态，在回调时，会在Query Parameter中回传该参数。开发者可以用这个参数验证请求有效性，也可以记录用户请求授权页前的位置。这个参数可用于防止跨站请求伪造（CSRF）攻击 |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 201101 | redirect_uri and client_id are required | api.Oauth.authorize  | 参数redirect_uri和client_id不可为空 |
| 201102 | Invalid response_type | api.Oauth.authorize  | 参数response_type无效 |
| 201103 | Invalid clientid | api.Oauth.authorize  | 参数clientid无效 |
| 201104 | redirect_uri and client_id are not matched | api.Oauth.authorize  | 参数redirect_uri和client_id不匹配 |
| 201105 | Authorization code required | api.Oauth.authorize  | 授权码不可为空 |

###### Example

> 
> 请求
> https://www.upare.com/apis/Oauth/authorize/?client_id=123050457758183&redirect_uri=http://www.example.com/response&response_type=code
> 
> 同意授权后会重定向
> http://www.example.com/response&code=CODE
> 

###### Return

```json
{"error_code":0,"api_uri":"apis/Oauth/authorize","message":"Successful","authorization_code":"a7cbae5f75ea7514ed963182d8ae43f71dcb1e88"}
```

> **authorization_code** (string): 用于提交到access_token接口的code

#### 2.1.2 access token

###### Usage: 获取access_token

**url:** /api/Oauth/accessToken

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| client_id | string | true | 申请应用时分配的AppKey |
| client_secret | string | true | 申请应用时分配的AppSecret |
| grant_type | string | true | 请求的类型，填写authorization_code/refresh_token/password |
| redirect_uri | string | true | 回调地址，需需与注册应用里的回调地址一致 |
| code | string | false | grant_type为authorization_code时，调用/apis/Oauth/authorize获得的code值 |
| refresh_token | string | false | grant_type为refresh_token时，通过授权得到的refhresh_token刷新access_token |
| username | string | false | grant_type为password时，系统用户名 |
| password | string | false | grant_type为password时，系统密码 |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 201201 | Invalid access_token | api.Oauth.accessToken  | 参数access_token无效 |
| 201202 | access_token is required | api.Oauth.accessToken  | 参数access_token不可为空 |
| 201203 | access_token expires or does not exist | api.Oauth.accessToken  | 参数access_token过期或不存在 |
| 201204 | API requests out of rate limit | api.Oauth.accessToken  | 接口请求超限 |
| 201205 | client_id client_secret grant_type redirect_uri are not matched | api.Oauth.accessToken  | 参数client_id, client_secret, grant_type, redirect_uri不匹配 |
| 201206 | Api permission denied | api.Oauth.accessToken  | 无权访问该接口 |
| 201207 | Cache access_token failed | api.Oauth.accessToken  | 缓存access_token失败 |
| 201208 | client_id client_secret are not matched | api.Oauth.accessToken  | 参数client_id和client_secret不匹配 |
| 201209 | Invalid refresh_token | api.Oauth.accessToken  | 参数refresh_token无效 |
| 201210 | client_id is required | api.Oauth.accessToken  | 参数client_id不可为空 |
| 201211 | client_secret is required | api.Oauth.accessToken  | 参数client_secret不可为空 |
| 201212 | Access username and password are required | api.Oauth.accessToken  | 用户名和密码不可为空 |
| 201213 | Invalid client_id | api.Oauth.accessToken  | 参数client_id无效 |
| 201214 | client_id and username are not matched | api.Oauth.accessToken  | 参数client_id和用户名不匹配 |
| 201215 | Invalid authorization code | api.Oauth.accessToken  | 授权码无效 |
| 201216 | Invalid grant_type | api.Oauth.accessToken  | 参数grant_type无效 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","access_token": "ACCESS_TOKEN","refresh_token": "REFRESH_TOKEN","expires_in": 1234,"remind_in":3600,"uid":12341234}
 ```
>  **access_token** (string): 用户授权的唯一票据，用于调用开放接口，同时也是第三方应用验证用户登录的唯一票据，第三方应用应该用该票据和自己应用内的用户建立唯一影射关系，来识别登录状态，不能使用本返回值里的UID字段来做登录识别。
>
>  **refresh_token** (string): grant_type为refresh_token时可通过refresh_token获得access_token。
>
>  **expires_in** (int): access_token的生命周期，单位是秒数。

#### 2.1.3 access token delete

###### Usage: 删除输入的access_token

**url:** /api/Oauth/delete

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| access_token | string | true | |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 201301 | Failed to delete access_token | api.Oauth.delete  | 删除access_token失败 |

#### 2.1.4 access token revoke

###### Usage: 撤销输入的access_token

**url:** /api/Oauth/revoke

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| access_token | string | true | |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 201401 | Failed to revoke access_token | api.Oauth.revoke  | |

### 2.2 User

#### 2.2.0 User common

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202001 | Account does not exist | api.User.common  | 账户不存在 |
| 202002 | The username already exists | api.User.common  | 该账户已存在 |
| 202003 | The email already exists | api.User.common  | 该邮件地址已存在 |
| 202004 | The phone number already exists | api.User.common  | 该电话号码已存在 |
| 202005 | The Nick name already exists | api.User.common  | 该昵称已存在 |
| 202006 | Invalid verification code | api.User.common  | 验证码无效 |
| 202007 | The sending interval of verification code is %d seconds | api.User.common  | 发送间隔为%d秒 |
| 202008 | Failed to send verification code | api.User.common  | 发送验证码失败 |
| 202009 | Invalid communication method | api.User.common  | 无效的通讯方式 |

#### 2.2.1 User login

###### Usage: 用户登录

**url:** /api/User/login

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| username | string | true | 用户名 |
| password | string | true | 密码 |
| captcha | string | true | /api/Image/captcha 接口产生的验证码 |
| remember | boolean | false | 是否永久登录 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","login_token":"TOKEN","uid":UID}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202101 | Not logged in yet | api.User.login  | 尚未登录 |
| 202102 | Username and password are required | api.User.login  | 用户名密码不可为空 |
| 202103 | Incorrect password | api.User.login  | 密码错误 |

#### 2.2.2 User register

###### Usage: 用户注册

**url:** /api/User/register

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| username | string | true | 用户名 |
| password | string | true | 密码 |
| email | string | true | 邮箱 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","login_token":"TOKEN","uid":UID}
 ```

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202202 | Failed to register | api.User.register  | 注册失败 |

#### 2.2.3 User settings

##### 2.2.3.1 Set avatar

###### Usage: 已经登录的用户上传头像

**url:** /api/User/setAvatar

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| avatar | file | true | 头像 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
##### 2.2.3.2 Set user basic information

###### Usage: 登录用户修改基本信息

**url:** /api/User/modifyBasicInfo

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| newpassword | string | false | 新密码 |
| confirm | string | relevance | 新密码确认,输入密码则为必填 |
| nickname | string | false | 昵称 |
| email | string | false | email |
| phone | string | false | phone |
| captcha | string | true | /api/Image/captcha 接口产生的验证码 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
##### 2.2.3.3 Set user detail information

###### Usage: 登录用户修改详细信息

**url:** /api/User/modifyDetailInfo

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| realname | string | false | 真实姓名 |
| idcard | string | false | 身份证号 |
| address | string | false | 地址 |
| gender | string | false | 性别 |
| nation | string | false | 民族 |
| postalcode | string | false | 邮编 |
| departmentcode | string | false | 部门编号 |
| majorcode | string | false | 专业编号 |
| leader | string | false | 领导姓名 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
##### 2.2.3.4 Set sns unbind

###### Usage: 设置社会化登录

**url:** /api/User/unbind

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| channel | string | false | 社会化登录的频道名称，如weibo,qq |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 100004 | Validation failed | system.common  | 上传验证失败（原因） |
| 202301 | Fail to upload avatar | api.User.setting  | 头像更新失败 |
| 202302 | Fail to unbind SNS account | api.User.setting  | 解绑社交网络账号失败 |
| 202303 | Failed to update user information | api.User.setting  | 更新用户信息失败 |
| 202304 | The old and new passwords are the same | api.User.setting  | 新旧密码相同 |
| 202305 | New and confirmed passwords are not the same | api.User.setting  | 新密码与确认密码不一致 |

#### 2.2.4 User SNS register

###### Usage: 用户通过社会化账号注册

Register a uid to bind the SNS account. if type=reg register a new user else if type=log bind a exist uid

**url:** /api/User/snsreg

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| username | string | true | 用户名 |
| password | string | true | 密码 |
| confirm | string | true | 确认密码 |
| email | string | true | 邮箱 |
| type | string | true | reg/log |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","login_token":"TOKEN","uid":UID}
 ```
#### 2.2.5 Validate unique username

###### Usage: 验证输入的用户名是否存在

**url:** /api/User/validateUniqueUsername

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| response_type | string | true | json OR code |
| username | string | true | 用户名 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202002 | The username already registered | api.User.common  | 用户名已存在 |

#### 2.2.6 Validate unique email

###### Usage: 验证输入的邮箱是否存在

**url:** /api/User/validateUniqueEmail

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| email | string | true | email |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202003 | The email already exists | api.User.common  | |

#### 2.2.7 Send email or mobile verification code

###### Usage: 向邮件或手机发送验证码

**url:** /api/User/sendcode

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| medium | string | false | 邮箱或手机号码 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202007 | The sending interval of verification code is %d seconds | api.User.common  | 发送间隔为%d秒 |
| 202101 | Not logged in yet | api.User.login  | 尚未登录 |

#### 2.2.8 Validate communication code

###### Usage: 验证通过邮件或手机收到的验证吗

**url:** /api/User/validateCommunicationCode

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| code | int | true | 邮箱或手机验证码 |
| medium | string | false | 邮箱或手机号码 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI"}
 ```
#### 2.2.9 User messages

###### Usage: 获取用户站内短信

If id=null list all the messages else read the message of the id

**url:** /api/User/message

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| id | int | false | 消息id |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","data":"MESSAGE"}
 ```

#### 2.2.10 User avatar

###### Usage: 获取不同尺寸的用户头像

Get user's photo in different sizes

**url:** /api/User/avatar

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| access_token | string | true | |
| uid/username | int/string | true |uid OR usernmae|
| size | int | true |420: 420 * 560, 413: 413 * 579|

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","avatar":"URL",,"width":W,"height":H,"version":V}
 ```
 or

 IMAGE

 ###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 202401 | The ID does not have an avatar | api.User.avatar  | 该ID无头像 |

#### 2.2.11 User uid

###### Usage: 通过用户名获取用户uid

Get user's uid

**url:** /api/User/uid

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| access_token | string | true | |
| username | string | true |  |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","uid":"UID"}
 ```

#### 2.2.12 User avatar upload api

###### Usage: 第三方通过uid上传头像

Upload user's avatar in the third app

**url:** /api/User/avatarUpload

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| access_token | string | true | |
| uid/username | int/string | true |uid OR usernmae|
| avatar | file | true | 头像 |

###### Return

 ```json
 {"error_code":0,"api_uri":"URI","message":"successful"}
 ```

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 100004 | Validation failed | system.common  | 上传验证失败（原因） |
| 202301 | Fail to upload avatar | api.User.setting  | 上传头像失败 |

### 2.3 Calendar

#### 2.3.1 Calendar events

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 203101 | No to-do events | api.Calendar.events  | 无代办事宜 |

#### 2.3.2 Calendar add event

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 203201 | Fail to add a event | api.Calendar.add  | 添加代办事项失败 |

#### 2.3.3 Calendar update event

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 203301 | Event id not specified | api.Calendar.update  | 未指定代办事项id |

### 2.4 Intercourse

#### 2.4.1 Friends

##### 2.4.1.1 List friends

###### Usage:

Get friends of logged uid

**url:** /api/User/friends

**method:** POST

##### 2.4.1.2 Make a friend

###### Usage:

Become friend to the uid input

**url:** /api/User/friendmake

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| fid | int | true | 朋友id |

##### 2.4.1.3 Delete a friend

###### Usage:

Remove friend to the uid input

**url:** /api/User/frienddel

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| fid | int | true | 朋友id |

##### 2.4.1.4 Friends recommend

###### Usage:

Recommend friends for logged user

**url:** /api/User/friendsrecommend

**method:** POST

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 204101 | No friends found | api.Intercourse.friend  | |
| 204102 | Failed to make friends | api.Intercourse.friend  | |
| 204103 | Failed to delete a friend | api.Intercourse.friend  | |
| 204104 | Failed to notify your friend | api.Intercourse.friend  | |

### 2.5 Content management

### 2.6 Image

#### 2.6.1 Captcha

###### Usage: 获取图片验证码

**url:** /api/Image/captcha

**method:** GET/POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| type | string | false | png/jpg/bmp |
| id | string | false | Captcha id for different scenarios  |

#### 2.6.2 Captcha verify

###### Usage: 图像验证码验证

**url:** /api/Image/captchaVerify

**method:** GET/POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| captcha | string | false | 验证码 |
| id | string | false | 不同id下的验证码 |
| flush | string | false | 默认值为yes,表示刷新后不重置，用于ajax接口 |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 200002 | Captcha verification failed | api.common  | 验证失败 |

#### 2.6.3 Encrypt a picture

###### Usage: 加密图像

**url:** /api/Image/chaos

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| image | string(base64) | true | 图像的base64编码 |
| password | int(6) | false | 密码 |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 206001 | Invalid input image | api.image.common  | 图像无效 |



#### 2.6.4 Decrypt a picture

###### Usage: 通过密码解密图像

**url:** /api/Image/order

**method:** POST

| Parameter | Type | REQUIRED | Remarks |
| - | - | - | - |
| image | string(base64) | true | 图像的base64编码 |
| password | int(6) | true | 密码 |

###### Error codes

| error_code (int) | Message(string) | URI | Remarks |
| :-: | :-: | :-: | :-: |
| 206001 | Invalid input image | api.image.common  | 图像无效 |