---
layout: default
title: "空间信息统计"
---

- [月度信息](#bucket-info)
- [查询空间使用详情](#select-space)
- [查询流量使用详情](#select-transfer)
- [查询请求数使用详情](#select-apicall)
- [查询各bucket空间使用状况](#buckets-space)
- [查询各bucket流量使用详情](#buckets-transfer)
- [查询各bucket请求数使用详情](#buckets-apicall)



<a name="bucket-info"></a> 
## 月度信息

> 摘要：按月份查询空间使用情况

API接口：

```
GET HOST/info?bucket=<string>&month=<string> HTTP/1.1
Authorization: <Authorization> 
```

其中，HOST为`http://api.qiniu.com/stat`,后文不再累述。

参数说明：

参数名称       | 说明
--------------|---------
bucket        | 指定bucket名，可选。指定则返回该用户该bucket下的信息，否则返回该用户的总信息。
month         | 指定月份，格式为`200601`
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    "space": <number>,           // 空间总量，单位Byte
    "space_avg": <number>,       // 空间平均量，单位Byte
    "apicall_get": <number>,     // get请求数，单位次
    "apicall_put": <number>,     // put请求数，单位次
    "transfer": <number>         // 流量总量，单位Byte
}
```

---

<a name="select-space"></a> 
## 查询空间使用详情 

> 摘要：按照指定的时间粒度查询空间的存储使用量

API接口：

```
GET HOST/select/space?bucket=<string>&from=<string>&to=<string>&p=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
bucket        | 指定bucket名，可选。指定则返回该用户该bucket下的信息，否则返回该用户的总信息
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
p             | 查询的颗粒度，为5min|day|month
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    time:
        [
             <number>,// 时间戳，单位为秒。靠前对齐。比如指定颗粒度为day，那该时间显示为当天0点0分0秒的时间戳
             ...
        ],
    data:
        [
            <number>,// 时间颗粒度上的使用量，单位byte
            ...
        ]
}
```

---

<a name="select-transfer"></a> 
## 查询流量使用详情

> 摘要：按照指定的时间范围查询空间的流量使用情况

API接口：

```
GET HOST/select/transfer?bucket=<string>&from=<string>&to=<string>&p=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
bucket        | 指定bucket名，可选。指定则返回该用户该bucket下的信息，否则返回该用户的总信息
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
p             | 查询的颗粒度，为5min|day|month
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    time:
        [
             <number>,// 时间戳，单位为秒。靠前对齐。比如指定颗粒度为day，那该时间显示为当天0点0分0秒的时间戳
             ...
        ],
    data:
        [
            <number>,// 时间颗粒度上的流量使用量，单位byte
            ...
        ]
}
```

---

<a name="select-apicall"></a>
## 查询请求数使用详情

> 摘要：按照指定的时间范围查询空间的API请求次数

API接口：

```
GET HOST/select/apicall?bucket=<string>&type=<string>&from=<string>&to=<string>&p=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
bucket        | 指定bucket名，可选。指定则返回该用户该bucket下的信息，否则返回该用户的总信息
type          | API请求类型，为get|put
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
p             | 查询的颗粒度，为5min|day|month
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    time:
        [
             <number>,// 时间戳，单位为秒。靠前对齐。比如指定颗粒度为day，那该时间显示为当天0点0分0秒的时间戳
             ...
        ],
    data:
        [
            <number>,// 时间颗粒度上的API请求次数
            ...
        ]
}
```

---

<a name="buckets-space"></a> 
## 查询各bucket空间使用状况

> 摘要：查询用户在指定的时间内所有空间的存储使用量

API接口：

```
GET HOST/buckets/space?from=<string>&to=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    bucket:
        [
             <string>,           // bucket名
             <string>,
             ...
        ],
    data:
        [
            <number>,           // 对应bucket在选择时间内的使用量
            ...
        ]
}
```

---


<a name="buckets-transfer"></a> 
## 查询各bucket流量使用详情

> 摘要：查询用户在指定的时间内所有空间的流量使用量

API接口：

```
GET HOST/buckets/transfer?from=<string>&to=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
uid           | 指定用户ID
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    bucket:
        [
             <string>,           // bucket名
             <string>,
             ...
        ],
    data:
        [
            <number>,           // 对应bucket在选择时间内的使用流量
            ...
        ]
}
```

---

<a name="buckets-apicall"></a> 
## 查询各bucket请求数使用详情

> 摘要：查询用户在指定的时间内所有空间的API请求数

API接口：

```
GET HOST/buckets/apicall?type=<string>&from=<string>&to=<string> HTTP/1.1
Authorization: <AccessToken>
```

参数说明：

参数名称       | 说明
--------------|---------
type          | API请求类型，为get|put
from          | 查询起始时间，格式为20060102
to            | 查询结束时间，格式为20060102
Authorization | 令牌，API请求的访问凭证，参考 [授权认证](http://docs.qiniu.com/api/v6/rs.html#digest-auth)

返回结果:

``` json
{
    bucket:
        [
             <string>,           // bucket名
             <string>,
             ...
        ],
    data:
        [
            <number>,           // 对应bucket在选择时间内的API请求次数
            ...
        ]
}
```
