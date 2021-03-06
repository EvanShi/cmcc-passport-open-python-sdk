中国移动通行证开放平台Python SDK
========

本项目是基于httplib开发的移动通行证平台Python SDK开发包；支持所有官方提供的API，可用于网站接入型应用和普通应用开发。

## 运行环境

* Python 2.6 / 2.7
* Flask(example) [链接][FLASK]

## 使用示例

### 引入SDK
```python
from cmcc import ChinaMobile
```

### 以Flask为例子
```python
oauth_config = {  \
    'oauth_consumer_key' : 'your oauth consumer key', \
    'oauth_app_secret'   : 'your oauth app secret' \
}
# 初始化
passport = ChinaMobile(oauth_config)
```


### 第一步: 获取Request token
```python
# 参数为oauth_callback url,可选
result = passport.get_request_token('http://127.0.0.1:5000/auth') 

# 将得到的未授权的Request Token以及对应的Request Token Secret存到session
session['oauth_token_secret'] = result['oauth_token_secret'][0]
session['oauth_token'] = result['oauth_token'][0]

# 设置使用得到的Request Token
passport.set_token(session['oauth_token'])
```


### 第二步: 请求用户授权Request Token
```python
return redirect(passport.get_authorize_url()) # 跳转授权
```


### 第三步: 使用授权后的Request Token换取Access Token
```python
# 设置token secret为第一步得到的oauth_token_secret
passport.set_token_secret(session['oauth_token_secret']) 
# 参数为第二步跳转回来的GET参数oauth_verifier
result = passport.get_access_token(request.args.get('oauth_verifier')) 

# 将得到的Access Token以及Access Token Secret存到session
session['oauth_access_token_secret'] = result['oauth_token_secret'][0]
session['oauth_access_token'] = result['oauth_token'][0]
```


### 第四步: 使用Access Token访问或修改受保护资源
```python
# 设置使用得到的Access Token
passport.set_token(session['oauth_access_token']) 
# 设置使用得到的Access Token Secret
passport.set_token_secret(session['oauth_access_token_secret']) 

# 查询个人信息api
query = passport.api_get('user/profile')
# HTTP 状态码
return query['status'] 
# 内容为 query['body'] 是一个Dict对象
# 所有方法 api_get('path') api_post('path',{}) api_put('path',{}) api_delete('path')
```

## 联系作者

Copyright (c) 2012 Situos Inc. ([http://www.situos.com][situos])

## License

The MIT license



[FlASK]: http://flask.pocoo.org/
[situos]: http://www.situos.com/