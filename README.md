<h1 style="text-align: center">HPU图书馆座位编号与API收集整理</h1>

<h3 style="text-align: center">野生API文档</h3>

- [前言](#前言)
- [过程](#过程)
- [接口](#接口)
  - [登陆](#登陆)
  - [用户信息](#用户信息)
  - [历史预约记录](#预约记录)
  - [违约记录](#违约记录这个我不太清楚我没有违约记录)
  - [当前预约信息](#当前预约)

### 前言

临近考试周，加上新图书馆的装修之豪华，图书馆座位变得越来越来越抢手，经常抢不过别人，所以打事看看能不能用个脚本去抢一下。

### 过程

我们学校座位预约可以在 PC 端的图书馆官网，也可以在微信公众号，也可以在一个叫 seat 的软件上。

PC 端每次登陆都需要验证码，搞不定；然后就用 fiddler 试了试抓包，公众号的话有个东西老是会过期，搞不定，发现 seat 好像简单多了。

手机使用`Packet Capture`软件进行抓包。

### 接口

#### 登陆

> http://seatlib.hpu.edu.cn/rest/auth

_请求方式：GET_

**url 参数：**

| 参数名   | 类型 | 内容 | 必要性 |
| -------- | ---- | ---- | ------ |
| username | str  | 学号 | 必要   |
| password | str  | 密码 | 必要   |

**json 回复：**

| 字段    | 类型 | 内容         | 备注             |
| ------- | ---- | ------------ | ---------------- |
| status  | str  | 登陆是否成功 | success：成功    |
| data    | obj  | 返回内容     | 失败时返回 null  |
| code    | str  | 状态码       | 成功时应该时是 0 |
| message | str  | 信息         | 成功时应该是空   |

`data`对象：

| 字段  | 类型 | 内容  | 备注           |
| ----- | ---- | ----- | -------------- |
| token | str  | token | 登陆成功时返回 |

#### 用户信息

> http://seatlib.hpu.edu.cn/rest/auth/v2/user

_请求方式：GET_

**url 参数：**

| 参数名 | 类型 | 内容  | 必要性 |
| ------ | ---- | ----- | ------ |
| token  | str  | token | 必要   |

**json 回复：**

| 字段    | 类型 | 内容     | 备注             |
| ------- | ---- | -------- | ---------------- |
| status  | str  | 是否成功 | success：成功    |
| data    | obj  | 返回内容 | 失败时返回 null  |
| code    | str  | 状态码   | 成功时应该时是 0 |
| message | str  | 信息     | 成功时应该是空   |

`data`对象：

| 字段           | 类型 | 内容                 | 备注 |
| -------------- | ---- | -------------------- | ---- |
| id             | num  | 未知                 |      |
| enable         | bool | 未知                 |      |
| username       | str  | 学号                 |      |
| status         | str  | 未知                 |      |
| lastLogin      | str  | 上次登陆时间         |      |
| checkdIn       | bool | 未知(应该是是否签到) |      |
| violationCount | num  | 违约记录数           |      |

#### 历史预约记录

> http://seatlib.hpu.edu.cn/rest/auth/v2/history/1/10

_请求方式：GET_

**url 参数：**

| 参数名 | 类型 | 内容  | 必要性 |
| ------ | ---- | ----- | ------ |
| token  | str  | token | 必要   |

**json 回复：**

| 字段    | 类型 | 内容     | 备注             |
| ------- | ---- | -------- | ---------------- |
| status  | str  | 是否成功 | success：成功    |
| data    | obj  | 返回内容 | 失败时返回 null  |
| code    | str  | 状态码   | 成功时应该时是 0 |
| message | str  | 信息     | 成功时应该是空   |

`data`对象：

| 字段         | 类型  | 内容             | 备注 |
| ------------ | ----- | ---------------- | ---- |
| reservations | array | 预约历史记录列表 |      |

#### 违约记录（这个我不太清楚，我没有违约记录）

> http://seatlib.hpu.edu.cn/rest/auth/v2/violations

_请求方式：GET_

**url 参数：**

| 参数名 | 类型 | 内容  | 必要性 |
| ------ | ---- | ----- | ------ |
| token  | str  | token | 必要   |

**json 回复：**

| 字段   | 类型  | 内容     | 备注            |
| ------ | ----- | -------- | --------------- |
| status | str   | 是否成功 | success：成功   |
| data   | array | 返回内容 | 失败时返回 null |

`data`数组列表：

#### 当前预约信息

> http://seatlib.hpu.edu.cn/rest/auth/v2/user/reservations

_请求方式：GET_

**url 参数：**

| 参数名 | 类型 | 内容  | 必要性 |
| ------ | ---- | ----- | ------ |
| token  | str  | token | 必要   |

**json 回复：**

| 字段    | 类型  | 内容     | 备注             |
| ------- | ----- | -------- | ---------------- |
| status  | str   | 是否成功 | success：成功    |
| data    | array | 返回内容 | 失败时返回 null  |
| code    | str   | 状态码   | 成功时应该时是 0 |
| message | str   | 信息     | 成功时应该是空   |

`data`数组中仅含有一个对象，该对象内容：

| 字段      | 类型 | 内容         | 备注                                               |
| --------- | ---- | ------------ | -------------------------------------------------- |
| id        | num  | 未知         |                                                    |
| receipt   | str  | 未知         |                                                    |
| onDate    | str  | 预约日期     |                                                    |
| seatID    | ine  | 座位 ID      |                                                    |
| status    | str  | 状态         |                                                    |
| location  | str  | 座位详细位置 | 如南校区第二图书馆 7 层 7 层北部阅览区，座位号 xxx |
| begin     | str  | 开始时间     |                                                    |
| end       | str  | 结束时间     |                                                    |
| userEnded | bool |              |                                                    |
| message   | str  | 信息         |                                                    |
