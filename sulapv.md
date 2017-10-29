# Sulapv 接口文档

## 约定

####HTTP

* 2xx 请求成功
* 401 token错误/为空/过期
* 400 参数错误

#### Request

* POST 请求时前端统一传递 JSON 格式参数 `Content-Type: application/json`
* 上传文件 `Content-Type: multipart/form-data`
* 必要的接口需要按要求带上 token，Header参数如下：

```http
Authorization: Bearer {{token}}
Accept: application/json
```

#### Response

* 接口返回正确时，只有 data 字段
* 请求错误时:

```json
{
  "errors": {
    "status": "SUL_0001",
    "title":  "",
    "detail": ""   
  }
}
```

#### 错误码

```
SUL_0001 验证邮箱失败
SUL_0002 邮箱或者密码错误
SUL_0003 企业名必填
SUL_0004 企业名不超过100字
SUL_0005 密码必填
SUL_0006 密码不少于6位
SUL_0007 邮箱必填
SUL_0008 邮箱格式不正确
SUL_0009 手机号必填
SUL_0010 姓必填
SUL_0011 姓不超过30字
SUL_0012 名必填
SUL_0013 名不超过30字
SUL_0014 项目名必填
SUL_0015 项目名应该在4-50字
SUL_0016 项目名重复
SUL_0017 新建项目地址不能为空
SUL_0018 上传logo的格式不正确
SUL_0019 上传logo的大小不符合
SUL_0020 评论必填
SUL_0021 邮箱注册重复
SUL_0098 404 NOT FOUND
SUL_0099 token过期/为空/未登录
```

## 接口

接口地址：

* 测试环境：http://server.sulapv.demo.chilunyc.com/api
* 生产环境：-

####登录注册

##### 登录接口

* user_type 为用户类型，`company`为企业，`individual`为个人用户

```json
POST /users/login

Body:
{
  "email": "panxianhai@gmail.com",
  "password": "123456"
}

Response:
{
  "data": {
    "token": ".....",
    "user_type": "individual",
    "email": "panxianhai@gmail.com",
    "phone": "18175150335",
    "company_name": "",
    "company_logo": "",
    "first_name": "xianhai",
    "last_name": "pan"
  }
}
```

##### 企业注册接口

```json
POST /users/business/register

Body:
{
  "company_name": "Alibaba",
  "password": "123456",
  "phone": "13800138000",
  "email": "contact@alibaba.com"
}

Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```

##### 个人注册接口

```json
POST /users/individual/register

Body:
{
  "email": "panxianhai@gmail.com",
  "phone": "18175150335",
  "last_name": "pan",
  "password": "123456",
  "first_name": "xianhai"
}

Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```

##### 激活接口

* email 和 token 参数为链接中的参数。

```json
POST /users/active

Body:
{
  "email": "panxianhai@gmail.com",
  "token": "1233333"
}

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 忘记密码接口

```json
POST /users/forget_password

Body:
{
    "email": "panxianhai@gmail.com"
}
```



####在线绘图

##### 最近项目接口

```json
GET /projects/recent

Body:

Response:
{
  "data": [
    {
      "id": 1,
      "project_name": "unnamed project 1",
      "address": "LA",
      "updated_at": "2017-09-09 12:12"
    }
  ]
}
```

##### 新增项目接口

```json
POST /projects

Body:
{
  "type": "111111",
  "name": "测试",
  "address": "雨花区",
  "component": "123",
  "roofs": [
    {
      "roof_name": "ABCVV",
      "azimuth": "123",
      "map_data": "123456",
      "slope": "2222"
    },
    {
      "roof_name": "ABCVV",
      "azimuth": "123",
      "map_data": "123456",
      "slope": "2222"
    }
  ]
}

Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```

##### 保存项目接口

```json
POST /project/{id}

Body:
{
  "thumb": "abc",
  "type": "residential",
  "name": "unamed project 1",
  "address": "LA",
  "component": "123",
  "roofs": [
    {
      "id": "",
      "roof_name": "ABCVV",
      "azimuth": "123",
      "map_data": "123456",
      "slope": "2222"
    },
    {
      "id": "1",
      "roof_name": "ABCVV",
      "azimuth": "123",
      "map_data": "123456",
      "slope": "2222"
    }
  ]
}

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 初始化项目接口

```json
GET /projects/{id}/init

Response:
{
  "data": {
    "id": 1,
    "project_img": "",
    "project_type": "111111",
    "project_name": "测试12333",
    "address": "雨花区",
    "component": "",
    "roofs": [
      {
        "id": 6,
        "roof_name": "123",
        "azimuth": "123",
        "slope": "12313",
        "map_data": "12312"
      },
      {
        "id": 7,
        "roof_name": "123",
        "azimuth": "123",
        "slope": "12313",
        "map_data": "12312"
      }
    ]
  }
}
```

#####删除项目接口

```json
POST /projects/{id}/delete

Response:
{
  "data": {
    "status": 1
  }
}
```

##### FAQ接口

```json
GET /faq

Body:

Response:
{
  "data": {
    "faq": "faq的富文本"
  }
}
```

##### 常用设备接口

```json
GET /equipments

Response:
{
  "data": {
    "module": [
      {
        "id": 2,
        "manufacturer": "测试",
        "equipment_model": "测试",
        "power": "100",
        "length": "100",
        "wide": "100"
      },
      {
        "id": 4,
        "manufacturer": "测试",
        "equipment_model": "测试",
        "power": "100",
        "length": "100",
        "wide": "100"
      }
    ],
    "inverter": [],
    "attachment": [],
    "racking": [
      {
        "id": 6,
        "manufacturer": "测试",
        "equipment_model": "测试",
        "power": "100",
        "length": "100",
        "wide": "100"
      },
      {
        "id": 7,
        "manufacturer": "测试",
        "equipment_model": "测试",
        "power": "100",
        "length": "100",
        "wide": "100"
      }
    ]
  }
}
```

##### project列表接口

```json
GET /projects

Response:
{
  "data": {
    "count": 1,
    "lists": [
      {
        "id": 1,
        "project_img": "",
        "project_name": "unamed project 1",
        "address": "LA",
        "updated_at": "2017-10-15 09:41"
      }
    ]
  }
}
```

##### package列表接口

```json
GET /packages

Response:
{
  "data": [
    {
      "id": 1,
      "name": "Site survey[2]",
      "price": 125,
      "childs": []
    },
    {
      "id": 2,
      "name": "Correction",
      "price": 0,
      "childs": []
    },
    {
      "id": 3,
      "name": "Prelimiary design",
      "price": 50,
      "childs": []
    },
    {
      "id": 4,
      "name": "Planset - Electrical",
      "price": 200,
      "childs": [
        {
          "id": 5,
          "pid": 4,
          "package": "Ground mount",
          "price": 50
        },
        {
          "id": 6,
          "pid": 4,
          "package": "Battery backup",
          "price": 50
        },
        {
          "id": 7,
          "pid": 4,
          "package": "Residential (Over 18kW DC)",
          "price": 50
        }
      ]
    },
    {
      "id": 8,
      "name": "Structural calculaitons",
      "price": 150,
      "childs": [
        {
          "id": 9,
          "pid": 8,
          "package": "Ground mount",
          "price": 50
        }
      ]
    },
    {
      "id": 10,
      "name": "Load Calcs\/Fault study",
      "price": 20,
      "childs": []
    },
    {
      "id": 11,
      "name": "Other forms",
      "price": 150,
      "childs": []
    },
    {
      "id": 12,
      "name": "Stamp[3]",
      "price": 200,
      "childs": []
    },
    {
      "id": 13,
      "name": "Revision[4]",
      "price": 50,
      "childs": []
    }
  ]
}
```

##### 获取Form接口

```json
GET /forms/{id}

Response:
{
  "data": {
    "id": 1,
    "name": "1",
    "phone": "",
    "license_type": "",
    "email": "",
    "address": "",
    "roof_type": "",
    "roof_pitch": "",
    "roof_frame_type": "",
    "roof_frame_size": "",
    "roof_frame_spacing": "",
    "roof_frame_a": "",
    "roof_frame_b": "",
    "roof_frame_c": "",
    "project_name": "",
    "project_type": "",
    "project_ahj": "",
    "project_address": "",
    "module_manufacturer": "",
    "module_model": "",
    "module_qty": "",
    "inverter_manufacturer": "",
    "inverter_model": "",
    "inverter_qty": "",
    "racking_manufacturer": "",
    "racking_model": "",
    "attachment_manufacturer": "",
    "attachment_model": "",
    "battery_backup_manufacturer": "",
    "battery_backup_model": "",
    "battery_backup_size": "",
    "battery_backup_controller": "",
    "utility_company": "",
    "grid_requirement": "",
    "main_service_panel_manufacturer": "",
    "main_service_panel_rating": "",
    "main_breaker_type": "",
    "main_breaker_rating": "",
    "sub_panel_manufacturer": "",
    "sub_panel_rating": "",
    "sub_panel_breaker_type": "",
    "sub_panel_breaker_rating": "",
    "images": "",
    "note": "",
    "created_at": null,
    "updated_at": null
  }
}
```

##### Form新建接口

* Body中的字段，字段列表同上面一个接口中返回的字段。除 `id, created_at, updated_at`

```json
POST /forms

Body:
{
  "name": "test name",
  "phone": "18175150335",
  "email": "panxianhaigmail.com"
}

Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```

##### From更新接口

* Body中的字段，字段列表同上面一个接口中返回的字段。除 `id, created_at, updated_at`

```json
POST /forms/{id}

Body:
{
  "email": "panxianhai@qq.com"
}

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 新建订单接口

* package为选择package的json

```json
POST /orders

Body:
{
  "package": "{}",
  "order_price": "180",
  "coupon": "1111111",
  "form_id": "1",
  "total_price": "200",
  "project_id": "1"
}

Response:
{
  "data": {
    "status": 1,
    "id": 2
  }
}
```

##### 获取优惠券信息接口

```json
GET /coupons/{coupon_no}

Response:
{
  "data": {
    "coupon_no": "123abc",
    "discount": "6.0"
  }
}
```



####个人中心

##### 修改企业名

```json
POST /companies/name

Body:
{
	"company_name":"123"
}

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 获取用户信息

```json
GET /users/info

Response:
{
  "data": {
    "user_type": "individual",
    "email": "panxianhai@gmail.com",
    "phone": "18175150335",
    "company_name": "",
    "company_logo": "",
    "first_name": "xianhai",
    "last_name": "pan"
  }
}
```

##### 系统通知接口

* order参数有两种：asc, desc

```json
GET /notices?page=1&page_size=10&order=asc

Response:
{
  "data": {
    "count": 1,
    "lists": [
      {
        "content": "213112313123123",
        "time": "12-12 12:12"
      }
    ]
  }
}
```

##### 邮箱验证接口

* 修改密码和修改邮箱的时候都需要进行邮箱验证

```json
# 修改密码邮箱验证
POST /users/reset_password
#修改邮箱邮箱验证
POST /users/reset_email

Response:
{
  "data": {
    "status": "1"
  }
}
```



##### 修改密码接口

```json
POST /users/change_password

Body:
{
  "password": "123456"
}
Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 修改邮箱接口

```json
POST /users/change_email

Body:
{
  "email": "panxianhai@163.com"
}
Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 订单列表接口

* type为订单的类型，分为四种：all, unpaid, process, finished 

```json
GET /orders?page=1&page_size=10&type=all

Response:
{
  "data": {
    "count": 1,
    "lists": [
      {
        "id": 1,
        "order_no": "222222",
        "package": "123123123123123",
        "total_price": 225,
        "order_status": 0
      }
    ]
  }
}
```

##### 订单详情接口

* form字段和获取form接口中返回的字段一致
* project和form字段可为空

```json
GET /orders/{id}

Response:
{
  "data": {
    "order_no": "201710153176",
    "package": [],
    "order_status": 0,
    "total_price": 200,
    "order_price": 180,
    "project": {
    	"id": 1,
      	"name": '123',
      	"thumb": 'a.jpg',
      	"address": 'eeee',
      	"component": 'eeeee'
    },
    "form": ""
  }
}
```



##### 文件列表接口

* order_id为订单的ID
* order:按照时间排序，有两个参数asc,desc

```json
GET /orders/{order_id}/files?page=1&page_size=10&order=desc

Response:
{
  "data": {
    "count": 3,
    "lists": [
      {
        "id": 1,
        "file_name": "1111",
        "file_size": "1.2",
        "created_at": "1970-01-01 08:00"
      },
      {
        "id": 2,
        "file_name": "1111",
        "file_size": "1.2",
        "created_at": "1970-01-01 08:00"
      },
      {
        "id": 3,
        "file_name": "1111",
        "file_size": "1.2",
        "created_at": "1970-01-01 08:00"
      }
    ]
  }
}
```

##### 文件详情接口

* order_id为订单的ID
* file_id为文件的ID

```json
GET /orders/{order_id}/files/{file_id}

Response:
{
  "data": {
    "file": {
      "file_name": "测试文件名",
      "file_url": "a.zip",
      "file_size": "2.3",
      "created_at": "2017-09-09 12:12:23"
    },
    "message": [
      {
        "id": 1,
        "name": "测试1",
        "message": "测试信息",
        "images": [
          "1.jpg",
          "2.jpg",
          "3.jpg"
        ],
        "created_at": "2017-12-12 10:12:20"
      },
      {
        "id": 2,
        "name": "测试1",
        "message": "测试信息",
        "images": [
          "1.jpg",
          "2.jpg",
          "3.jpg"
        ],
        "created_at": "2017-12-12 10:12:20"
      },
      {
        "id": 3,
        "name": "panxianhai",
        "message": "测试信息",
        "images": [
          "public\/ndOBKW7urPRc5ocSvgnWgRt4DNCrvjWs9kwkp7sJ.jpeg",
          "public\/6zpg7Z4sR6hNL0oi3T1BZH9wzj4SNwpZNuZehtAr.jpeg"
        ],
        "created_at": "2017-10-17 16:04:30"
      }
    ]
  }
}
```

##### 提交评论接口

```json
POST /messages
Content-Type: multipart/form-data

Body:
image[]=图片列表
message=测试信息
file_id=1

Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```



##### 我的项目列表接口

```json
GET /users/projects

Response:
{
  "data": {
    "count": 1,
    "lists": [
      {
        "project_name": "111",
        "address": "22222",
        "updated_at": "2017-09-09 12:12"
      }
    ]
  }
}
```

##### 个人资料修改接口

```json
POST /users/individual

Body:
{
  "phone": "18175150336",
  "first_name": "hevin",
  "last_name": "panda"
}
Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 企业LOGO修改接口

```json
POST /companies/logo

Content-Type: multipart/form-data

Body:
company_logo=文件

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 企业联系人新增接口

```json
POST /persons

Body:
{
  "name": "hevin",
  "email": "panxianhai@gmail.com",
  "phone": "18175150335"
}
Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```



##### 企业联系人删除接口

```json
POST /persons/{person_id}/delete

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 企业联系信息新增接口

```json
POST /contacts

Body:
{
  "email": "panxianhai@163.com",
  "phone": "18175150335",
  "license": "chlunyc",
  "name": "hevin",
  "address": "my address"
}
Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```



##### 企业联系信息删除接口

```json
POST /contacts/{contact_id}/delete

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 企业设备新增接口

```json
POST /equipments

Body:
{
  "type": "module",
  "model": "123123",
  "manufacturer": "111111"
}
Response:
{
  "data": {
    "status": "1",
    "id": 1
  }
}
```

##### 企业设备删除接口

```json
POST /equipments/{equipment_id}/delete

Response:
{
  "data": {
    "status": "1"
  }
}
```

##### 企业详情接口

```json
GET /companies

Response:
{
  "data": {
    "email": "panxianhai@gmail.com",
    "company_name": "company name",
    "company_logo": "http://www.a.com/a.jpg",
    "contacts": [],
    "people": [],
    "module": [],
    "inverter": [],
    "racking": [],
    "attachment": []
  }
}
```

##### 文件上传接口

```json
POST /images/upload
Content-Type: multipart/form-data

Body:
image[]=文件

Response:
{
  "data": [
    "public\/XlfvRoOR8Ifj5XQQTrq2hnjnotSsxT4XbQ7tcCrC.jpeg"
  ]
}
```



