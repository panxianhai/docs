##通用

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

## 会员登录

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

* 字段还不完善，有待完善

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
    "bm": "1006", // 加盟店编码
    "mc": "邵阳店", // 加盟店名称
    "mc_all": "1006-邵阳店",
    "id_jbr": 0,
    "lxr": "",
    "user_id": 20, // 用户ID
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIiwianRpIjoiNGYxZzIzYTEyYWEifQ.eyJpc3MiOiJodHRwOlwvXC93d3cuamp0bS5vcmciLCJhdWQiOiRwOlwvXC93d3cuamp0bS5vcmciLCJqdGkiOiI0ZjFnMjNhMTJhYSIsImlhdCI6MTUxNDMzOTM0MiwibmJmIjoxNTE0MzM5MzQyLCJleHAiOjE1MTQzNzUzNDIsInVpZCI6MjB9."
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

* 需要更新的字段有待完善

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

## 商品管理

### 分类列表

* id_gsjg: 0为总部分类，其他为加盟商分类

```json
GET /goods/categories

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 1,
      "code": "000001",
      "name": "测试分类1",
      "id_gsjg": 0
    },
    {
      "id": 2,
      "code": "900501",
      "name": "加盟商分类1",
      "id_gsjg": 9005
    }
  ]
}
```

### 商品列表

* 按照分类获取和分页待完善。

```json
GET /goods/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "count": 2, // 商品总数量
    "list": [
      {
        "id": 19978,
        "bm": "110510349", // 编码
        "mc": "工字架\/R60", //名称
        "syjx": "R60", // 适配机型
        "dj_ls": "390.00", //零售价
        "dj_pf3": "325.00" //进货价
      },
      {
        "id": 19977,
        "bm": "110630366",
        "mc": "引导轮螺栓\/XG806",
        "syjx": "XG806",
        "dj_ls": "2.00",
        "dj_pf3": "1.20"
      }
    ]
  }
}
```

### 新增商品

```json
POST /goods/create

Body:
{
  "goods_name": "测试商品",
  "bm": "JMD900500004", // 编码自动生成
  "cost_price": "100",
  "retail_price": "150",
  "unit": "箱",
  "category": "1",
  "models": "适配机型",
  "bar_code": "111222333",
  "specification": "规格型号",
  "notes": "备注",
  "brand": "品牌",
  "images": "1.jpg;2.jpg;3.jpg",
  "init_num": "100",
  "stock_warning": "1",
  "goods_status": "1"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": []
}
```



### 编辑商品

* 只编辑库存数量

```json
POST /goods/edit

Body:
{
  "init_num": "100"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": []
}
```

