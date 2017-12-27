### 通用

* 请求地址：`http://192.168.31.183:2017`,登陆接口示例：`http://192.168.31.183:2017/user/login`
* Header部分需要的参数：

```http
Accept: application/json
Authorization: token...
```

* 返回的格式统一为：

```json
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

error：0 为返回正常，1 为请求失败，-1参数错误，1000为需要重新登录。

### 图片上传接口

```json
POST /file/upload

Header:
Content-Type: multipart/form-data; charset=utf-8; 

Body:
upfile[]=a.jpg

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    "uploads\/images\/da7c47dbb1325477ccfefab035cecec8f546f822.png",
    "uploads\/images\/87078741d76fe6568023e3e10c07d4263431ab2e.png"
  ]
}
```

### 登录接口

```json
POST /user/login

Body:
{
  "username": "1103001",
  "password": "123456"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 7,
    "bm": "1006",
    "mc": "邵阳店",
    "mc_all": "1006-邵阳店",
    "id_jbr": 0,
    "lxr": "",
    "user_id": 20,
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIiwianRpIjoiNGYxZzIzYTEyYWEifQ.eyJpc3MiOiJodHRwOlwvXC93d3cuamp0bS5vcmciLCJhdWQiOiJodHRwOlwvXC93d3cuamp0bS5vcmciLCJqdGkiOiI0ZjFnMjNhMTJhYSIsImlhdCI6MTUxNDMzOTM0MiwibmJmIjoxNTE0MzM5MzQyLCJleHAiOjE1MTQzNzUzNDIsInVpZCI6MjB9."
  }
}
```

### 修改头像

```json
POST /user/avatar

Body:
{
  "avatar": "1.jpg"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": []
}
```

### 修改资料

```json
POST /user/edit

Body:
{
  "mc": "名称",
  "tel_sj": "18175150333"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": []
}
```

