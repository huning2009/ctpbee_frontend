# API 接口文档

## 1. API接口说明

- 接口基准地址：`http://192.168.31.30:5000/`
- 使用socket.io推送消息
- 服务端已开启 CORS 跨域支持
- API V1 认证统一使用 Token 认证
- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌
- 使用 HTTP Status Code 标识状态
- 数据返回格式统一使用 JSON

### 1.1 支持的请求方法

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
- DELETE（DELETE）：从服务器删除资源。
- HEAD：获取资源的元数据。
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

### 1.2 通用返回状态说明

| *状态码* | *含义*                | *说明*                                              |
| -------- | --------------------- | --------------------------------------------------- |
| 200      | OK                    | 请求成功                                            |
| 201      | CREATED               | 创建成功                                            |
| 204      | DELETED               | 删除成功                                            |
| 400      | BAD REQUEST           | 请求的地址不存在或者包含不支持的参数                |
| 401      | UNAUTHORIZED          | 未授权                                              |
| 403      | FORBIDDEN             | 被禁止访问                                          |
| 404      | NOT FOUND             | 请求的资源不存在                                    |
| 422      | Unprocesable entity   | [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误 |
| 500      | INTERNAL SERVER ERROR | 内部错误                                            |

------

## 2 登录

### 2.1 登录验证接口

- 请求路径：login
- 请求方法：post
- 请求参数

| 参数名   | 参数说明 | 备注     |
| -------- | -------  | -------- |
| userid | 用户名   | 不能为空 |
| password | 密码     | 不能为空 |
| authorization | 授权码     | 不能为空(默认为:000000) |
| interface | 接口     | 不能为空 |
| brokerid| 期货公司编号     | 不能为空 |
| appid | 产品名称     | 不能为空 |
| auth_code | 认证码     | 不能为空 |
| td_address | 交易前置     | 不能为空 |
| md_address | 行情前置     | 不能为空 |

- 响应参数

| 参数名   | 参数说明    | 备注            |
| -------- | ----------- | --------------- |
| success  | 请求是否成功 |                 |
| data     | token       |基于 jwt 的令牌   |
| msg      | 提示        |                 |

- 响应数据

```json
{
    "success":true,
    "data":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzM",
    "msg": "登录成功"
}
```

## 3.账户

### 3.1账户数据获取

- socket事件名:account
  
- 响应参数

| 参数名   | 参数说明    | 备注            |
| -------- | ----------- | --------------- |
| type     | 事件名称     |                 |
| data     | 数据         |                 |

- 响应数据

```json
{
  "type": "account",
  "data":[
    {key:"accountid",value:"089131"},
    {key:"available",value:689195.3399999999},
    {key:"balance",value:1188174.5899999999},
    {key:"frozen",value:0},
    {key:"gateway_name",value:"ctp"},
    {key:"local_account_id",value:"ctp.089131"},
  ]
}
```

## 4.行情

### 4.1获取品种列表

- 请求路径: market
- 请求方法: put
- 响应数据

```json
  {
    data: "",
    msg: "更新合约列表完成",
    success: true
  }
```

### 4.2订阅

- 请求路径: market
- 请求方法: post
- 请求参数
  
  |参数名   |参数说明   |备注                  |
  |--------|-----------|----------------------|
  |symbol  |品种名称   |不能为空(eg:ag1901.DCE)|

- 响应数据

```json
  {
    data: "",
    msg: "订阅a1911.DCE成功",
    success: true
  }
```

### 4.3获取订阅列表

- socket事件名: tick
  
- 响应参数

| 参数名   | 参数说明    | 备注            |
| -------- | ----------- | --------------- |
| type     | 事件名称     |                 |
| data     | 数据         |                 |

- 响应数据

```json
{
  type:"tick",
  data:{
      ask_price_1: 4424
      ask_price_2: 0
      ask_price_3: 0
      ask_price_4: 0
      ask_price_5: 0
      ask_volume_1: 2
      ask_volume_2: 0
      ask_volume_3: 0
      ask_volume_4: 0
      ask_volume_5: 0
      average_price: 64728.63636363636
      bid_price_1: 4216
      bid_price_2: 0
      bid_price_3: 0
      bid_price_4: 0
      bid_price_5: 0
      bid_volume_1: 2
      bid_volume_2: 0
      bid_volume_3: 0
      bid_volume_4: 0
      bid_volume_5: 0
      datetime: "2019-10-31 11:30:00.500000"
      exchange: "SHFE"
      gateway_name: "ctp"
      high_price: 4371
      last_price: 4326
      last_volume: 0
      limit_down: 4064
      limit_up: 4583
      local_symbol: "ag1911.SHFE"
      low_price: 4285
      name: "白银1911"
      open_interest: 314
      open_price: 4290
      pre_close: 4321
      pre_settlement_price: 4324
      symbol: "ag1911"
      volume: 132
  }
}
```
