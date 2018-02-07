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
    "user_id": 20, // 用户ID
    "account": "1402005", // 账号
    "username": "hnldxh", // 名称
    "avatar": "", // 头像
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

### 系统预置参数

```json
GET /user/params

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "gh_ddrk_default_gys_allow_arrear": "0", //采购业务默认的供应商是否允许欠款
    "gh_ddrk_default_gys_id": null, //采购业务默认的供应商id
    "xs_ck_default_kh_allow_arrear": "0", // 销售单默认的客户是否允许欠款
    "xs_ck_default_kh_id": null, // 销售单默认的客户id
    "zbgh_ddrk_default_gys_id": "174" //总部采购业务默认的供应商id
  }
}
```



## 商品管理

### 分类列表

* zb 为总部分类
* jmd为加盟店分类

```json
GET /goods/categories

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "zb": [
      {
        "id": 1,
        "code": "000001",
        "name": "测试分类1",
        "id_gsjg": 0
      }
    ],
    "jmd": [
      {
        "id": 2,
        "code": "900501",
        "name": "加盟商分类1",
        "id_gsjg": 25
      }
    ]
  }
}
```

### 获得加盟店自己的分类

```json
GET /goods/category

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 2,
      "code": "900501",
      "name": "加盟商分类1",
      "id_gsjg": 25
    }
  ]
}
```

### 获得总部分类

```json
GET /goods/cat_zb

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 2,
      "code": "900501",
      "name": "加盟商分类1",
      "id_gsjg": 25
    }
  ]
}
```

### 

### 加盟店新增分类

```json
POST /goods/add_category

Body:
{
  "name": "加盟店测试分类"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 9,
    "name": "加盟店测试分类"
  }
}
```

### 商品列表

* filter和ID两个参数都可选，两个都为空的时候获取全部商品
* filter有两种参数，即选择总部分类和加盟商分类的时候，总部分类参数为zb，加盟商分类参数为jmd
* id参数需要在filter参数有的前提下才生效，为分类的实际ID

```json
GET /goods/index?filter=zb&id=1

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
        "dj_pf3": "325.00", //进货价
        "cover": "",
        "kc": 123
      },
      {
        "id": 19977,
        "bm": "110630366",
        "mc": "引导轮螺栓\/XG806",
        "syjx": "XG806",
        "dj_ls": "2.00",
        "dj_pf3": "1.20",
        "cover": "",
        "kc": 123
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
  "init_price": 100,
  "init_total": 200,
  "stock_warning_low": 1,
  "stock_warning_high": 20,
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
  "id_sp": "12334",
  "goods_name": "测试商品",
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
  "init_price": 100,
  "init_total": 200,
  "stock_warning_low": 1,
  "stock_warning_high": 20,
  "goods_status": "1"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": []
}
```

### 商品详情

```json
GET /goods/show/1

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": "1",
    "goods_name": "测试",
    "bm": "11223344",
    "cost_price": "225.00",
    "retail_price": "225.00",
    "unit": "箱",
    "category_id": "1",
    "category": "1",
    "models": "",
    "bar_code": "112233333",
    "specification": "",
    "notes": "",
    "brand": "",
    "images": "",
    "init_num": "",
    "init_price": "",
    "init_total": "",
    "stock_warning_low": "",
    "stock_warning_high": "",
    "goods_status": ""
  }
}
```

### 单位列表

```json
GET /goods/unit

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 1,
      "dw": "个"
    },
    {
      "id": 2,
      "dw": "套"
    },
    {
      "id": 3,
      "dw": "把"
    },
    {
      "id": 4,
      "dw": "件"
    },
    {
      "id": 5,
      "dw": "支"
    },
    {
      "id": 6,
      "dw": "箱"
    },
    {
      "id": 7,
      "dw": "瓶"
    },
    {
      "id": 8,
      "dw": "根"
    },
    {
      "id": 9,
      "dw": "块"
    },
    {
      "id": 10,
      "dw": "条"
    },
    {
      "id": 11,
      "dw": "匝"
    },
    {
      "id": 12,
      "dw": "米"
    },
    {
      "id": 13,
      "dw": "桶"
    },
    {
      "id": 14,
      "dw": "副"
    },
    {
      "id": 15,
      "dw": "付"
    },
    {
      "id": 16,
      "dw": "车付"
    },
    {
      "id": 17,
      "dw": "罐"
    },
    {
      "id": 18,
      "dw": "袋"
    },
    {
      "id": 19,
      "dw": "台"
    },
    {
      "id": 20,
      "dw": "颗"
    },
    {
      "id": 21,
      "dw": "片"
    },
    {
      "id": 22,
      "dw": "只"
    },
    {
      "id": 23,
      "dw": "盒"
    },
    {
      "id": 24,
      "dw": "张"
    },
    {
      "id": 25,
      "dw": "盘"
    },
    {
      "id": 26,
      "dw": "对"
    }
  ]
}
```

### 获取品牌列表

```json
GET /goods/brands

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 57,
      "bm": "0057",
      "mc": "华伦"
    },
    {
      "id": 58,
      "bm": "0058",
      "mc": "雪企"
    },
    {
      "id": 59,
      "bm": "0059",
      "mc": "金冷"
    },
    {
      "id": 60,
      "bm": "0060",
      "mc": "上海三电贝尔"
    },
    {
      "id": 61,
      "bm": "0061",
      "mc": "NOK"
    },
    {
      "id": 62,
      "bm": "0062",
      "mc": "博朗"
    },
    {
      "id": 63,
      "bm": "0063",
      "mc": "东洋（DYB）"
    },
    {
      "id": 64,
      "bm": "0064",
      "mc": "徐工正厂"
    },
    {
      "id": 65,
      "bm": "0065",
      "mc": "工兵"
    },
    {
      "id": 66,
      "bm": "0066",
      "mc": "劲戈"
    },
    {
      "id": 67,
      "bm": "0067",
      "mc": "南丰"
    },
    {
      "id": 68,
      "bm": "0068",
      "mc": "泉永QY"
    },
    {
      "id": 69,
      "bm": "0069",
      "mc": "三阳"
    },
    {
      "id": 70,
      "bm": "0070",
      "mc": "亚工"
    },
    {
      "id": 71,
      "bm": "0071",
      "mc": "再生"
    },
    {
      "id": 72,
      "bm": "0072",
      "mc": "荣盛"
    },
    {
      "id": 73,
      "bm": "0073",
      "mc": "申亿"
    },
    {
      "id": 74,
      "bm": "0074",
      "mc": "REDYIN"
    },
    {
      "id": 75,
      "bm": "0075",
      "mc": "加德士"
    },
    {
      "id": 76,
      "bm": "0076",
      "mc": "雪飞"
    },
    {
      "id": 1,
      "bm": "0001",
      "mc": "默认品牌"
    },
    {
      "id": 2,
      "bm": "0002",
      "mc": "壳牌"
    },
    {
      "id": 3,
      "bm": "0003",
      "mc": "山推小松"
    },
    {
      "id": 4,
      "bm": "0004",
      "mc": "北京日石"
    },
    {
      "id": 5,
      "bm": "0005",
      "mc": "天津日石"
    },
    {
      "id": 6,
      "bm": "0006",
      "mc": "长城"
    },
    {
      "id": 7,
      "bm": "0007",
      "mc": "统一"
    },
    {
      "id": 8,
      "bm": "0008",
      "mc": "洋马"
    },
    {
      "id": 9,
      "bm": "0009",
      "mc": "康明斯"
    },
    {
      "id": 10,
      "bm": "0010",
      "mc": "五十铃"
    },
    {
      "id": 11,
      "bm": "0011",
      "mc": "小松"
    },
    {
      "id": 12,
      "bm": "0012",
      "mc": "卡特"
    },
    {
      "id": 13,
      "bm": "0013",
      "mc": "沃尔沃"
    },
    {
      "id": 14,
      "bm": "0014",
      "mc": "久保田"
    },
    {
      "id": 15,
      "bm": "0015",
      "mc": "日本川崎"
    },
    {
      "id": 16,
      "bm": "0016",
      "mc": "韩国先进"
    },
    {
      "id": 17,
      "bm": "0017",
      "mc": "韩国东明"
    },
    {
      "id": 18,
      "bm": "0018",
      "mc": "第一油压"
    },
    {
      "id": 19,
      "bm": "0019",
      "mc": "第星"
    },
    {
      "id": 20,
      "bm": "0020",
      "mc": "帝人（纳博特斯克）"
    },
    {
      "id": 21,
      "bm": "0021",
      "mc": "奥盖尔"
    },
    {
      "id": 22,
      "bm": "0022",
      "mc": "豪斯科"
    },
    {
      "id": 23,
      "bm": "0023",
      "mc": "力士乐"
    },
    {
      "id": 24,
      "bm": "0024",
      "mc": "派克"
    },
    {
      "id": 25,
      "bm": "0025",
      "mc": "派芬"
    },
    {
      "id": 26,
      "bm": "0026",
      "mc": "贵阳永青"
    },
    {
      "id": 27,
      "bm": "0027",
      "mc": "苏州蓝博"
    },
    {
      "id": 28,
      "bm": "0028",
      "mc": "赫斯曼"
    },
    {
      "id": 29,
      "bm": "0029",
      "mc": "奔宇"
    },
    {
      "id": 30,
      "bm": "0030",
      "mc": "凯勒"
    },
    {
      "id": 31,
      "bm": "0031",
      "mc": "神州"
    },
    {
      "id": 32,
      "bm": "0032",
      "mc": "咸工"
    },
    {
      "id": 33,
      "bm": "0033",
      "mc": "浙东"
    },
    {
      "id": 34,
      "bm": "0034",
      "mc": "钻石"
    },
    {
      "id": 35,
      "bm": "0035",
      "mc": "江苏恒力"
    },
    {
      "id": 36,
      "bm": "0036",
      "mc": "徐州徐液"
    },
    {
      "id": 37,
      "bm": "0037",
      "mc": "上柴"
    },
    {
      "id": 38,
      "bm": "0038",
      "mc": "潍柴"
    },
    {
      "id": 39,
      "bm": "0039",
      "mc": "玉柴"
    },
    {
      "id": 40,
      "bm": "0040",
      "mc": "青州"
    },
    {
      "id": 41,
      "bm": "0041",
      "mc": "洛阳钢锋"
    },
    {
      "id": 42,
      "bm": "0042",
      "mc": "杭州前进"
    },
    {
      "id": 43,
      "bm": "0043",
      "mc": "河南风神"
    },
    {
      "id": 44,
      "bm": "0044",
      "mc": "贵州前进"
    },
    {
      "id": 45,
      "bm": "0045",
      "mc": "唐纳森"
    },
    {
      "id": 46,
      "bm": "0046",
      "mc": " 上海弗列加"
    },
    {
      "id": 47,
      "bm": "0047",
      "mc": "浙江海德曼"
    },
    {
      "id": 48,
      "bm": "0048",
      "mc": "广东耀泰"
    },
    {
      "id": 49,
      "bm": "0049",
      "mc": "黎明液压"
    },
    {
      "id": 50,
      "bm": "0050",
      "mc": "贺德克"
    },
    {
      "id": 51,
      "bm": "0051",
      "mc": "瓦尔塔"
    },
    {
      "id": 52,
      "bm": "0052",
      "mc": "骆驼"
    },
    {
      "id": 53,
      "bm": "0053",
      "mc": "达克罗"
    },
    {
      "id": 54,
      "bm": "0054",
      "mc": "罗特艾德"
    },
    {
      "id": 55,
      "bm": "0055",
      "mc": "方圆"
    },
    {
      "id": 56,
      "bm": "0056",
      "mc": "统力"
    }
  ]
}	
```

### 扫描条码获得商品信息

```json
GET /goods/scan/111222333

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 3278,
    "bm": "111310018",
    "mc": "徐工挖机(18L)抗磨液压油46号",
    "syjx": "",
    "dj_ls": 348,
    "dj_pf3": 0,
    "cover": ""
  }
}
```



## 客户管理

### 客户列表

```json
GET /customer/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "customers": [
      {
        "id": 2049,
        "bm": "407210",
        "mc": "潘大大2",
        "mc_all": "407210-潘大大2",
        "lxr": "测试",
        "tel": "18175150335",
        "dz": "测试地址",
        "balance": "0.00"
      }
    ],
    "count": 1,
    "money": 0
  }
}
```

### 新增客户

```json
POST /customer/create

Body:
{
  "phone": "18175150335",
  "address": "测试地址",
  "remarks": "备注测试",
  "mc": "潘大大",
  "armny": "1000",
  "tel": "021-00002222",
  "name": "测试"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 2172,
    "name": "潘大大1245"
  }
}
```

### 删除客户

```json
POST /customer/delete

Body:
{
  "kh_id": "2049"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 客户详情

```json
GET /customer/show/2049

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 2049,
    "mc": "潘大大",
    "lxr": "潘大大",
    "tel": "18175150335",
    "tel_gs": "021-00002222",
    "dz": "测试地址",
    "bz": "备注测试",
    "armny": 0
  }
}
```

### 编辑客户

```json
POST /customer/edit

Body:
{
  "kh_id": 2049,
  "phone": "18175150335",
  "address": "测试地址",
  "remarks": "备注测试",
  "mc": "潘大大",
  "tel": "021-00002222",
  "name": "测试"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

## 供应商管理

### 获取总部供应商

0: 总部供应商 1:自己管理的供应商

```json
GET /supplier/get/0

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 174,
      "bm": "274",
      "mc": "9999总部供应商"
    }
  ]
}
```



### 供应商列表

```json
GET /supplier/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "Suppliers": [
      {
        "id": 2049,
        "bm": "407210",
        "mc": "潘大大2",
        "mc_all": "407210-潘大大2",
        "lxr": "测试",
        "tel": "18175150335",
        "dz": "测试地址",
        "balance": 0
      }
    ],
    "count": 1,
    "money": 0
  }
}
```

### 新增供应商

```json
POST /supplier/create

Body:
{
  "phone": "18175150335",
  "address": "测试地址",
  "remarks": "备注测试",
  "mc": "潘大大",
  "paymny": "1000",
  "tel": "021-00002222",
  "name": "测试"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 删除供应商

```json
POST /supplier/delete

Body:
{
  "gys_id": "2049"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 供应商详情

```json
GET /supplier/show/2049

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": 2049,
    "mc": "潘大大",
    "lxr": "潘大大",
    "tel": "18175150335",
    "tel_gs": "021-00002222",
    "dz": "测试地址",
    "bz": "备注测试",
    "paymny": 0
  }
}
```

### 编辑供应商

```Json
POST /supplier/edit

Body:
{
  "gys_id": 2049,
  "phone": "18175150335",
  "address": "测试地址",
  "remarks": "备注测试",
  "mc": "潘大大",
  "tel": "021-00002222",
  "name": "测试"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```
## 总部采购订单

### 总部采购订单列表

```json
GET /order/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "begin": "2017-12-17",
    "end": "2018-01-17",
    "lists": [
      {
        "dh": "CD0242017112800109",
        "mc": "9999总部供应商",
        "je_hs": "1780.00",
        "je_bhs": "1780.00",
        "rq": "2018-01-17 10:37:06.720",
        "gh_dh": null,
        "flag_sh": 0
      }
    ]
  }
}
```

### 新增总部采购订单(草稿)

```json
POST /order/create

Body:
{
  "id_gys": "174",
  "bz": "12333333",
  "pay_type": 1,
  "goods": [
    {
      "id_sp": "12333",
      "num": "2",
      "dj_pf3": "110",
      "jzdw_mc": "桶"
    },
    {
      "id_sp": "12334",
      "num": "10",
      "dj_pf3": "120",
      "jzdw_mc": "桶"
    }
  ]
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "CD0222017112800034",
    "user": "张家界加盟店",
    "createtime": "2018-02-12 12:22:11"
  }
}
```

### 更新总部采购订单(草稿)

```json
POST /order/update

Body:
{
  "id_gys": "174",
  "dh": "CD0222017112800034",
  "bz": "12333333",
  "pay_type": 2,
  "goods": [
    {
      "id_sp": "12333",
      "num": "2",
      "dj_pf3": "120",
      "jzdw_mc": "桶"
    }
  ]
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 总部采购提交订单

```json
POST /order/submit

Body:
{
  "bz": "",
  "dh": "",
  "pay_type": 0,
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_gys": "174"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "CD0242017112800110",
    "user": "hnldxh",
    "createtime": "2018-02-12 12:22:11"
  }
}
```



### 总部采购订单详情

```json
GET /order/show/CD0222017112800034 (单号)

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id_gys": 174,
    "dh": "CD0222017112800034",
    "je_bhs": 240,
    "je_hs": 240,
    "count": 10,
    "user": "张家界加盟店",
    "state": 1,
    "createtime": "2018-02-12 12:34:21",
    "payment": 1,
    "paymode": 1，
    "total_fee"： "123.22",
    "paytime": "2018-02-12 12:34:21",
    "goods": [
      {
        "id_sp": 12333,
        "sl": 2,
        "dj_bhs": 120,
        "dj_hs": 120,
        "je_bhs": 240,
        "je_hs": 240,
        "mc": "现代水温过热报警器D6BR D6BRT",
        "syjx": ""
      }
    ]
  }
}
```

### 总部采购订单搜索

```json
POST /order/search

Body:
{
  "dh": "",
  "end_time": "2018-02-01",
  "begin_time": "2017-12-12"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "dh": "CD0222017112800005",
      "mc": "9999总部供应商",
      "je_hs": 220,
      "je_bhs": 220,
      "rq": "2018-01-09 09:45:33",
      "flag_sh": "0"
    },
    {
      "dh": "CD0222017112800006",
      "mc": "9999总部供应商",
      "je_hs": 220,
      "je_bhs": 220,
      "rq": "2018-01-09 09:45:34",
      "flag_sh": "0"
    },
    {
      "dh": "CD0222017112800007",
      "mc": "9999总部供应商",
      "je_hs": 220,
      "je_bhs": 220,
      "rq": "2018-01-09 09:45:35",
      "flag_sh": "0"
    }
  ]
}
```

### 查询销售出库单号

```json
GET /order/getck/CD0332018011900018

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "ck_dh": "PF0172018011800039"
  }
}
```

### 延迟收货

```json
POST /order/delay

Body:
{
  "dh": "CD0332018012500003"
}

Response:
{
  "error": 1,
  "message": "您已经申请过延期了",
  "data": {}
}
```

### 凭证列表

```json
GET /voucher/show/CD0332018012500003

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "voucher_no": "222222",
      "shipper": "tester",
      "filename": [
        "1.jpg",
        "2.jpg"
      ],
      "uploadtime": "2018-12-12 12:11:11"
    }
  ]
}
```



## 总部收货单

### 结算方式

```json
GET /receive/cash_type

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 1,
      "mc": "微信",
      "isdefault": 1
    },
    {
      "id": 2,
      "mc": "银行卡",
      "isdefault": 0
    }
  ]
}
```

### 新增总部采购收货单（草稿）

```json
POST /receive/create

Body:
{
  "bz": "",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_gys": "174",
  "bdsf": "1400", // 实付金额
  "dh_dd": "CD0222017112800050" // 出库单单号
}

Response:{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "CR0222017112800007",
    "user": "张家界加盟店"
  }
}

```

### 更新总部收货单(草稿)

```json
POST /receive/update

Body:
{
  "bz": "",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_gys": "174",
  "bdsf": "1400",
  "dh_dd": "CD0222017112800050",
  "dh": "CR0222017112800011"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 总部收货单详情

```Json
GET /receive/show/CR0222017112800011

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id_gys": 174,
    "dh": "CR0222017112800011",
    "je_bhs": "1205.00",
    "je_hs": "1205.00",
    "je_sf": "1400.00",
    "goods": [
      {
        "id_sp": 19978,
        "sl": 1,
        "dj_bhs": "325.00",
        "dj_hs": "325.00",
        "je_bhs": "325.00",
        "je_hs": "325.00",
        "mc": "工字架\/R60",
        "syjx": "R60"
      },
      {
        "id_sp": 2000,
        "sl": 1,
        "dj_bhs": "390.00",
        "dj_hs": "390.00",
        "je_bhs": "715.00",
        "je_hs": "715.00",
        "mc": "F481CACF101005-1050 软管总成  XE370、XE210C",
        "syjx": "XE370、210C"
      },
      {
        "id_sp": 3000,
        "sl": 1,
        "dj_bhs": "490.00",
        "dj_hs": "490.00",
        "je_bhs": "1205.00",
        "je_hs": "1205.00",
        "mc": "XG21.5-CD 铲斗油缸修理包  XE215CA",
        "syjx": "XE215CA"
      }
    ],
    "count": 3,
    "user": "张家界加盟店"
  }
}
```

### 总部收货单列表

```json
GET /receive/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "begin": "2017-12-17",
    "end": "2018-01-17",
    "lists": [
      {
        "dh": "CR0242017112800028",
        "mc": "9999总部供应商",
        "je_hs": "325.00",
        "je_bhs": "325.00",
        "rq": "2018-01-17 14:25:56.413",
        "flag_sh": "0"
      }
    ]
  }
}
```

### 销售出库单引用列表

```json
GET /receive/reference

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "dh": "PF0222017042400001",
      "id_gsjg": 23,
      "mc": "张家界市服务站",
      "rq": "2017-04-24 14:56:25",
      "je_hs": 73869
    },
    {
      "dh": "PF0222017042800002",
      "id_gsjg": 23,
      "mc": "张家界市服务站",
      "rq": "2017-04-28 15:26:20",
      "je_hs": 154534
    },
    {
      "dh": "PF0172017100900003",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-10-09 09:41:46",
      "je_hs": 4320
    },
    {
      "dh": "PF0172017102800090",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-11-01 00:00:00",
      "je_hs": 27880
    },
    {
      "dh": "PF0172017102800091",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-11-01 00:00:00",
      "je_hs": 54000
    },
    {
      "dh": "PF0172017102800093",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-11-01 00:00:00",
      "je_hs": 27000
    },
    {
      "dh": "PF0172017103100095",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-11-01 00:00:00",
      "je_hs": 32410
    },
    {
      "dh": "PF0172017111500039",
      "id_gsjg": 18,
      "mc": "湖南宏维机甲",
      "rq": "2017-11-15 14:34:28",
      "je_hs": 84000
    }
  ]
}
```

### 销售出库单详情

```json
GET /receive/details/PF0222017042400001

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id_gys": 174,
    "dh": "PF0222017042400001",
    "je_bhs": 73869,
    "je_hs": 73869,
    "count": 329,
    "je_sf": 73869,
    "user": "张家界加盟店",
    "goods": [
      {
        "id_sp": 15858,
        "sl": 1,
        "dj_bhs": 120,
        "dj_hs": 120,
        "je_bhs": 120,
        "je_hs": 120,
        "mc": "空气滤芯(天亦蓝TA-8615)",
        "syjx": ""
      },
      {
        "id_sp": 15862,
        "sl": 1,
        "dj_bhs": 70,
        "dj_hs": 70,
        "je_bhs": 70,
        "je_hs": 70,
        "mc": "空气滤芯(天亦蓝TA-8625)",
        "syjx": ""
      }
    ]
  }
}
```

### 收货单确认收货

```json
POST /receive/submit

Body:
{
  "bz": "",
  "id_gys": "174",
  "bdsf": "1400",
  "cash_type": "1"， // 结算账户
  "dh_dd": "FR0222017112800050",
  "dh": "CR0222017112800011",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ]
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "CD0242017112800110",
    "user": "hnldxh"
  }
}
```

##销售单

### 客户列表

```Json
GET /sale/customers

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": 227,
      "mc": "陕西分公司"
    }
  ]
}
```

### 销售单列表

```json
GET /sale/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "begin": "2017-12-17",
    "end": "2018-01-17",
    "lists": [
    {
      "dh": "JJCR0222017112878176",
      "mc": "潘楚明",
      "je_hs": "1205.00",
      "je_bhs": "1205.00",
      "rq": "2018-01-13 06:14:51",
      "flag_sh": 0
    },
    {
      "dh": "JJCR02220171128",
      "mc": "潘楚明",
      "je_hs": "1205.00",
      "je_bhs": "1205.00",
      "rq": "2018-01-12 23:05:47",
      "flag_sh": 0
    }
  ]
  }
}
```

### 新增销售单

```json
POST /sale/create

Body:
{
  "bz": "",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_kh": "122",
  "bdsf": "1400",
  "dh_dd": "CD0222017112800050",
  "zhyf": "123",
  "cash_type": 5,
  "discount": "80",
  "erasemny": "123.21"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "JJCR0222017112878176",
    "user":"张家界加盟店"
  }
}
```

### 更新销售单

```json
POST /sale/update

Body:
{
  "bz": "测试备注",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_kh": "122",
  "bdsf": "1400",
  "dh_dd": "CD0222017112800050",
  "dh": "JJCR0222017112878176",
   "zhyf": "123",
  "cash_type": 5,
  "discount": "80",
  "erasemny": "123.21"
}
REsponse:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 销售单详情

```json
GET /sale/show/JJCR0222017112878176

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id_kh": 122,
    "dh": "JJCR0222017112878176",
    "je_bhs": "1205.00",
    "je_hs": "1205.00",
    "bdsf": "100.00",
    "bz": "beizhu",
    "zhyf": '1233', //折后应付
    'zkl': '89', //折扣率
    'erasemny':"25.2", // 抹零金额
    "cash_type": "4",
    "cash_type_name": "4",
    "goods": [
      {
        "id_sp": 19978,
        "sl": 1,
        "dj_bhs": "325.00",
        "dj_hs": "325.00",
        "je_bhs": "325.00",
        "je_hs": "325.00",
        "mc": "工字架\/R60",
        "syjx": "R60"
      },
      {
        "id_sp": 2000,
        "sl": 1,
        "dj_bhs": "390.00",
        "dj_hs": "390.00",
        "je_bhs": "390.00",
        "je_hs": "390.00",
        "mc": "F481CACF101005-1050 软管总成  XE370、XE210C",
        "syjx": "XE370、210C"
      },
      {
        "id_sp": 3000,
        "sl": 1,
        "dj_bhs": "490.00",
        "dj_hs": "490.00",
        "je_bhs": "490.00",
        "je_hs": "490.00",
        "mc": "XG21.5-CD 铲斗油缸修理包  XE215CA",
        "syjx": "XE215CA"
      }
    ],
    "count": 3,
    "user": "张家界加盟店"
  }
}
```

### 提交销售单

```json
POST /sale/submit

Body:
{
  "bz": "测试备注2",
  "goods": [
    {
      "dj_pf3": "325.00",
      "id_sp": "19978",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "390.00",
      "id_sp": "2000",
      "jzdw_mc": "桶",
      "num": "1"
    },
    {
      "dj_pf3": "490.00",
      "id_sp": "3000",
      "jzdw_mc": "桶",
      "num": "1"
    }
  ],
  "id_kh": "122",
  "bdsf": "1400",
  "zhyf": "123", // 折后应付
  "dh_dd": "CD0222017112800050",
  "dh": "JJCR0222017112878176",
  "cash_type": "1",
  "erasemny": '25.6', // 抹零金额
  "discount": 80 // 折扣率
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "CD0242017112800110",
    "user": "hnldxh"
  }
}
```

## 库存管理

### 库存查询

sort参数：mr 默认，bm编码，sl数量，je金额

```Json
GET /stock/index?sort=bm

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "kc_num": ".0000000",
    "kc_money": ".0000000",
    "lists": [
      {
        "id": "15855",
        "barcode": "2211121078504",
        "mc": "空气滤芯(天亦蓝TA-8506)",
        "sl_qm": ".0000000",
        "je_qm_hs": ".0000000",
        "cover": ""
      },
      {
        "id": "15855",
        "barcode": "2211051034908",
        "mc": "工字架\/R60",
        "sl_qm": ".0000000",
        "je_qm_hs": ".0000000",
        "cover": ""
      }
    ]
  }
}
```

### 库存详情

```json
GET /stock/details/18899

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": "15855",
    "bm": "111210785",
    "barcode": "2211121078504",
    "mc": "空气滤芯(天亦蓝TA-8506)",
    "sl_qm": "0.00",
    "je_qm_hs": "0.00",
    "je_in_hs": "0.00",
    "dj_cb_hs": "0.00",
    "dj_ls": "203.00",
    "cover": ""
  }
}
```

### 库存告警

```json
GET /stock/warning

Response:

```

## 打印订单

### 打印订单

type参数：order采购订单，receive 收货单，sale销售单

```json
POST /printing/index

Body:
{
  "dh": "JJCR0242017112800001",
  "type": "sale"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "JJCR0242017112800001",
    "je_bhs": "1205.00",
    "je_hs": "1205.00",
    "bz": "测试备注2",
    "rq": "2018-01-19 09:54:07",
    "name": "潘楚明",
    "bdsf": "1400.00",
    "erasemny"： "0.00"
    "goods": [
      {
        "id_sp": 19978,
        "sl": 1,
        "dj_bhs": "325.00",
        "dj_hs": "325.00",
        "je_bhs": "325.00",
        "je_hs": "325.00",
        "mc": "工字架\/R60",
        "syjx": "R60"
      },
      {
        "id_sp": 2000,
        "sl": 1,
        "dj_bhs": "390.00",
        "dj_hs": "390.00",
        "je_bhs": "390.00",
        "je_hs": "390.00",
        "mc": "F481CACF101005-1050 软管总成  XE370、XE210C",
        "syjx": "XE370、210C"
      },
      {
        "id_sp": 3000,
        "sl": 1,
        "dj_bhs": "490.00",
        "dj_hs": "490.00",
        "je_bhs": "490.00",
        "je_hs": "490.00",
        "mc": "XG21.5-CD 铲斗油缸修理包  XE215CA",
        "syjx": "XE215CA"
      }
    ],
    "count": 3,
    "user": "娄底新化服务站",
    "zhyf": "1205.00",
    "zkl": "100.00",
    "bdqk": "0.00",
    "cash_type": ""
  }
}
```

## 应收报表

### 应收报表首页

```json
GET /receivable/index

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "qk_count": 0,
    "qk_total": "0.00",
    "jy_count": 3,
    "jy_total": "-2042.00",
    "lists": [
      {
        "id": 1,
        "mc": "马学文",
        "lxr": "123",
        "tel": "456",
        "balance": "-100.00"
      },
      {
        "id": 2,
        "mc": "廖志",
        "lxr": "",
        "tel": "",
        "balance": "-987.00"
      },
      {
        "id": 3,
        "mc": "宋征酌",
        "lxr": "",
        "tel": "",
        "balance": "-955.00"
      }
    ]
  }
}
```

### 应用报表客户

```json
GET /receivable/customer/2080?from=2018-01-01&to2018-02-01

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "qc": "0.00",
    "qm": "0.00",
    "ys": "460.00",
    "ss": "460.00",
    "yh": "0.00",
    "lists": [
      {
        "mc": "",
        "dh": "SKDR0332017112800001",
        "rq": "2018-01-19 14:03:55",
        "yhje": "", // 优惠金额
        "je": "20.00", // 实收金额
        "ys": "20.00", // 应收金额
        "ysye": "0.00", // 应收余额
        "bill_type": "skd"
      },
      {
        "mc": "",
        "dh": "JJCR0332017112800002",
        "rq": "2018-01-19 07:07:53",
        "yhje": "",
        "je": "20.00",
        "ys": "20.00",
        "ysye": "0.00",
        "bill_type": "xsd"
      },
      {
        "mc": "",
        "dh": "JJCR0332017112800003",
        "rq": "2018-01-19 07:12:56",
        "yhje": "",
        "je": "420.00",
        "ys": "420.00",
        "ysye": "0.00",
        "bill_type": "xsd"
      }
    ]
  }
}
```

### 收款单据

```json
GET /receivable/skd/2080?from=2018-01-01&to2018-02-01

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "qc": "0.00",
    "qm": "0.00",
    "ys": "460.00",
    "ss": "460.00",
    "yh": "0.00",
    "lists": [
      {
        "mc": "",
        "dh": "SKDR0332017112800001",
        "rq": "2018-01-19 14:03:55",
        "yhje": "",
        "je": "20.00",
        "ys": "20.00",
        "ysye": "0.00",
        "bill_type": "skd"
      }
    ]
  }
}
```

### 欠款单据

```json
GET /receivable/xsd/2080?from=2018-01-01&to2018-02-01

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "qc": "0.00",
    "qm": "0.00",
    "ys": "460.00",
    "ss": "460.00",
    "yh": "0.00",
    "lists": [
      {
        "mc": "",
        "dh": "JJCR0332017112800002",
        "rq": "2018-01-19 07:07:53",
        "yhje": "",
        "je": "20.00",
        "ys": "20.00",
        "ysye": "0.00",
        "bill_type": "xsd"
      },
      {
        "mc": "",
        "dh": "JJCR0332017112800003",
        "rq": "2018-01-19 07:12:56",
        "yhje": "",
        "je": "420.00",
        "ys": "420.00",
        "ysye": "0.00",
        "bill_type": "xsd"
      }
    ]
  }
}
```

## 销售报表

### 销售报表统计

Time: 0,1,2,3

```json
GET /salebill/summary?time=0

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "yh": "83.00", // 折扣额
    "je": "460.00", //销售额
    "dd_sl": 1,  //订单数量
    "sp_sl": 3 // 商品数量
  }
}
```

### 商品列表

```json
GET /salebill/goods?time=0

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id": "15855",
      "mc": "空气滤芯(天亦蓝TA-8506)",
      "bm": "111210785",
      "sl": 3,
      "je_hs": "609.00"
    },
    {
      "id": "16054",
      "mc": "柴油滤芯（天亦蓝TF-2501）",
      "bm": "111210984",
      "sl": 4,
      "je_hs": "80.00"
    },
    {
      "id": "16282",
      "mc": "回油滤芯（天亦蓝TH-6739）",
      "bm": "110210296",
      "sl": 37,
      "je_hs": "6938.00"
    }
  ]
}
```

### 客户列表

```json
GET /salebill/customer?time=0

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "id_kh": "2170",
      "mc": "零售客户",
      "num": 15,
      "je_hs": "3452.00",
      "je_yh": "369.10"
    },
    {
      "id_kh": "2173",
      "mc": "张飞的店",
      "num": 1,
      "je_hs": "543.00",
      "je_yh": "83.00"
    },
    {
      "id_kh": "2174",
      "mc": "刘备的店",
      "num": 1,
      "je_hs": "197.00",
      "je_yh": "0.00"
    },
    {
      "id_kh": "2176",
      "mc": "司马的店",
      "num": 2,
      "je_hs": "1005.00",
      "je_yh": "135.00"
    },
    {
      "id_kh": "2177",
      "mc": "曹操的店",
      "num": 3,
      "je_hs": "1385.00",
      "je_yh": "208.00"
    },
    {
      "id_kh": "2178",
      "mc": "abc",
      "num": 4,
      "je_hs": "1045.00",
      "je_yh": "208.00"
    }
  ]
}
```

## 收款单

### 客户欠款详情

* id_kh 为客户的ID

```json
GET /skd/show/{id_kh}

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "id": "1",
    "mc": "马学文",
    "balance": ".00000000"
  }
}
```

### 新增收款单

```json
POST /skd/create

Body:
{
  "id_jsfs": "1", // 结算方式
  "id_kh": "1",
  "je": "100",
  "yhje": "0",
  "bz": "www",
  "imgs": [
    "1.jpg",
    "2.jpg"
  ]
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "dh": "JJSK0332018012500005"
  }
}
```

### 收款单详情

```json
GET /skd/details/JJSK0332018012500004

Response:
{
  "error": 0,
  "message": "success",
  "data": {
    "kh_mc": "马学文",
    "pay_type": "现金",
    "je": "100.00",
    "yhje": "0.00",
    "rq": "2018-01-30 10:27:00",
    "bz": "www",
    "dh": "JJSK0332018012500004"
  }
}
```

### 编辑收款单

```json
POST /skd/edit

Body:
{
  "id_jsfs": "1",
  "je": "100",
  "yhje": "0",
  "bz": "www2",
  "imgs": [
    "1.jpg",
    "2.jpg"
  ],
  "rq": "2018-01-01 12:12:12",
  "dh": "JJSK0332018012500006"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 删除收款单

```json
POST /skd/delete

Body:
{
  "dh": "JJSK0332018012500006"
}
Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

## 反馈问题

### 新增反馈

```json
POST /feedback/create

Body:
{
  "ques_desc": "描述信息",
  "filename": [
    "a.jpg",
    "b.jpg"
  ],
  "dd_dh": "20180102111111"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 取消反馈

```json
POST /feedback/cancel

Body:
{
  "dh": "20180207140633574417"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 修改反馈

```json
POST /feedback/update

Body:
{
  "ques_desc": "描述信息22",
  "filename": [
    "a.jpg",
    "b.jpg"
  ],
  "dh": "20180207140633574417"
}

Response:
{
  "error": 0,
  "message": "success",
  "data": {}
}
```

### 反馈列表

```json
GET /feedback/index

Response:
{
  "error": 0,
  "message": "success",
  "data": [
    {
      "dh": "20180207141417611468",
      "state": 1,
      "dd_dh": "20180102111111",
      "id_gsjg": "34",
      "createtime": "02-07 14:14",
      "ques_desc": "描述信息",
      "creater": "hnhyhs",
      "filename": [
        "a.jpg",
        "b.jpg"
      ],
      "readques_flag": 0,
      "replytime": "",
      "reply_info": "",
      "replyer": "",
      "canceltime": "",
      "canceller": "",
      "cancelflag": 0,
      "readreplay_flag": 0
    }
  ]
}
```

