<h1 style="text-align: center">HPU图书馆座位编号与API收集整理</h1>

<h3 style="text-align: center">野生API文档</h3>

[toc]

#### 前言

临近考试周，加上新图书馆的装修之豪华，图书馆座位变得越来越来越抢手，经常抢不过别人，所以打事看看能不能用个脚本去抢一下。

#### 过程

我们学校座位预约可以在PC端的图书馆官网，也可以在微信公众号，也可以在一个叫seat的软件上。

PC端每次登陆都需要验证码，搞不定；然后就用fiddler试了试抓包，公众号的话有个东西老是会过期，搞不定，发现seat好像简单多了。

手机使用`Packet Capture`软件进行抓包。

#### 接口

##### 登陆

> http://seatlib.hpu.edu.cn/rest/auth

*请求方式：GET*

**json回复：**

| 字段    | 类型 | 内容         | 备注           |
| :------ | :--- | :----------- | :------------- |
| status  | str  | 登陆是否成功 | success：成功  |
| data    | obj  | 返回内容     | 失败时返回null |
| code    | str  |              |                |
| message | str  |              |                |

`data`对象：

| 字段  | 类型 | 内容  | 备注           |
| ----- | ---- | ----- | -------------- |
| token | str  | token | 登陆成功时返回 |

