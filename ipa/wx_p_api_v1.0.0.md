# 微效客户端接口文档（MA-AS）

[TOC]



## 1 前言

### 1.1 版本信息 

| 版本          | 修订人  | 修订日期   | 概述     |
| ------------- | ------- | ---------- | -------- |
| v1.0.0 修订版 | lynwood | 2019-05-06 | 接口规范 |
|               |         |            |          |
|               |         |            |          |

### 1.2 文档简介

本文档适用于新华新媒SMP事业部微效平台的设计及研发,提供微效(安卓/ios)客户端作为规范接口标准文件.

文档目的在于规范接口调用,明确客户端和服务端接口开发的一致性和准确性.

也是新华新媒各级人员对微效平台工程设计、网络运营、管理、维护等方面的技术依据.

### 1.3  专业术语

| 变量        | 说明             | 示例                 |
| ----------- | ---------------- | -------------------- |
| ${ip}       | 服务端域名       | http://192.168.30.69 |
| ${port}     | 服务端端口       | 8080                 |
| ${basePath} | 服务项目请求基准 | smpApi               |

### 1.4  缩略语

| 缩写 | 说明                  |
| ---- | --------------------- |
| MA   | (MerchantAPP) 商户APP |
| AS   | (ApiServer) 服务端    |
| NOUN | (Noun) 名词对照       |

* 其他业务名词说明参见文档: [<名词统一列表-AS-NOUN.md>](./名词统一列表-AS-NOUN.md)

### 1.5 统一返回值数据模型

| 参数名称 | 字段类型 | 示例                                         | 描述                                                |
| -------- | -------- | -------------------------------------------- | --------------------------------------------------- |
| code     | int      | 200                                          | 服务端状态码  参照 [1.6状态码说明](#1.6 状态码说明) |
| status   | String   | OK                                           | 状态说明                                            |
| message  | String   | 登录成功                                     | 提示信息                                            |
| result   | Object   | {"token":"eed38c1b7e794ffcba4309b5fd294a5b"} | 数据包体                                            |

示例(ResultModel) :

```json
{
    "status": "OK",
    "result": "xxxxxxxxx",
    "message": "登录成功",
    "code": 200
}
```

### 1.6 状态码说明

| 状态码(code) | 状态说明(status)      | 原因              | 备注 |
| ------------ | --------------------- | ----------------- | ---- |
| 200          | OK                    | 正常              |      |
| 400          | Bad Request           | 操作失败          |      |
| 401          | Unauthorized          | 未登录/登录过期   |      |
| 403          | Forbidden             | 访问拒绝/没有权限 |      |
| 404          | Not Found             | 找不到资源        |      |
| 500          | Internal Server Error | 服务器错误        |      |
|              |                       |                   |      |



## 2 接口概述

------

### 2.1 工具接口

---

#### 2.1.1 发送绑定手机号短信验证码接口

##### 2.1.1.1 接口协议描述

* 接口描述: 发送短信验证码(6位纯数字随机码) 以绑定注册时手机号码  有效时长10分钟
* 接口协议: HTTP POST
* 数据格式: JSON
* 接口调用方向: MA -> AS
* URL Mapping: http://${ip}:${port}/${basePath}/common/sendSmsCode

##### 2.1.1.2 请求要素说明

| 参数名  | 数据类型 | 长度 | 注释                    | 可为空 |
| ------- | -------- | ---- | ----------------------- | ------ |
| mobile  | String   | 11   | 手机号                  | N      |
| smsType | int      | 1    | 短信类型 1-验证码类短信 | N      |

* 注

  ```java
  手机号 校验正则 : ^1([38][0-9]|4[579]|5[0-3,5-9]|6[6]|7[0135678]|9[89])\d{8}$
  ```

##### 2.1.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释     | 是否空 |
| ------ | -------- | ---- | -------- | ------ |
| result | String   |      | 返回状态 | 否     |

##### 2.1.1.4 案例

* 请求示例:

  ```json
  {
      "mobile":"17722221111",
      "smsType":1
  }
  ```

* 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "发送成功"
  }
  ```

* 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "今日验证码次数超出限制"
  }
  ```

---

#### 2.1.2 时间校准接口

##### 2.1.2.1 接口协议描述

- 接口描述: 校准当前时间
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/common/calibrationTime

##### 2.1.2.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

##### 2.1.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释                                         | 是否空 |
| ------ | -------- | ---- | -------------------------------------------- | ------ |
| result | String   |      | 返回字符串格式时间 : yyyy-MM-dd HH:mm:ss:SSS | 否     |

##### 2.1.2.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "2019-06-15 13:18:44:672",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

### 2.2 用户登录模块

---

#### 2.2.1 判断邀请码是否正确接口

##### 2.2.1.1 接口协议描述

- 接口描述: 判断邀请码是否正确接口 查看邀请码是否正确/被使用
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/invit/checkInvitCodeStauts

##### 2.2.1.2 请求要素说明

| 参数名    | 数据类型 | 长度 | 注释   | 可为空 |
| --------- | -------- | ---- | ------ | ------ |
| invitCode | String   | 6    | 邀请码 | N      |

- 注

  ```java
  邀请码 校验正则 : ^\d{6}$
  ```

##### 2.2.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.2.1.4 案例

- 请求示例:

  ```json
  {
      "invitCode":"654321"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "可使用"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "验证码已使用/验证码不正确"
  }
  ```

---

#### 2.2.2 注册接口

##### 2.2.2.1 接口协议描述

- 接口描述: 商户注册接口  需要与微信认证 
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/orgAccount/register

##### 2.2.2.2 请求要素说明

| 参数名     | 数据类型 | 长度  | 注释                   | 可为空 |
| ---------- | -------- | ----- | ---------------------- | ------ |
| invitCode  | String   | 6     | 邀请码                 | N      |
| mobile     | String   | 11    | 手机号                 | N      |
| smsCode    | String   | 6     | 短信验证码             | N      |
| password   | String   | 32    | 密码 需要md5(p,32)加密 | N      |
| openid     | String   | 0-30  | 微信openid             | N      |
| nickname   | String   | 0-30  | 微信昵称               | N      |
| headimgurl | String   | 0-200 | 微信头像地址           | N      |
| sex        | int      | 1     | 性别 1：男,2：女       | Y      |
| country    | String   | 0-20  | 国家                   | Y      |
| province   | String   | 0-20  | 省份                   | Y      |
| city       | String   | 0-20  | 城市                   | Y      |

- 注

  ```java
  邀请码/短信验证码 校验正则 : ^\d{6}$
  手机号 校验正则 : ^1([38][0-9]|4[579]|5[0-3,5-9]|6[6]|7[0135678]|9[89])\d{8}$
  ```

##### 2.2.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释             | 是否空 |
| ------ | -------- | ---- | ---------------- | ------ |
| result | String   |      | 返回注册操作状态 | 否     |

##### 2.2.2.4 案例

- 请求示例:

  ```json
  {
  	"invitCode": "654321",
  	"mobile": "17722221111",
  	"smsCode": "012345",
  	"password": "e10adc3949ba59abbe56e057f20f883e",
  	"openid": "o_wxwuPBuLpMLhEuzPcBOr6Bg7sI",
  	"nickname": "Wx_NickName",
  	"headimgurl"：
  	"http://wx.qlogo.cn/mmopen/PiajxSqBRaEL7hBAAkJ50Dp9k3d1ID1xufMw2qPb7XHJ9GR0TiaosCVDSicmhX9ibeQFtgbib7XQaia25VnLJkUsviavg/0",
  	"sex": 1,
  	"country": "阿拉伯联合酋长国",
  	"province": "迪拜",
  	"city": "迪拜"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "注册成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "用户已存在"
  }
  ```

---

#### 2.2.3 登录接口

##### 2.2.3.1 接口协议描述

- 接口描述: 登陆接口 （商户登陆,学员登陆）   
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/auth/login

##### 2.2.3.2 请求要素说明

| 参数名      | 数据类型 | 长度 | 注释                   | 可为空 |
| ----------- | -------- | ---- | ---------------------- | ------ |
| mobile      | String   | 11   | 登陆手机号码           | N      |
| password    | String   | 32   | 密码 需要md5(p,32)加密 | N      |
| accountType | int      | 1    | 账户类型 1-商户 2-学员 | N      |

- 注

  ```java
  登陆手机号码 校验正则 : ^1([38][0-9]|4[579]|5[0-3,5-9]|6[6]|7[0135678]|9[89])\d{8}$
  ```

##### 2.2.3.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名          | 数据类型 | 长度 | 注释         | 是否空 |
| --------------- | -------- | ---- | ------------ | ------ |
| result          | Object   |      | 返回登陆信息 | 否     |
| result.roleId   | int      | 3    | 角色id       | 否     |
| result.roleName | String   | 5-30 | 角色描述     | 否     |
| result.token    | String   |      | 用户凭证     | 否     |

* 注

  ```java
  登陆成功后将返回token缓存到客户端本地 并且之后api相关交互将token 放入请求Header的accessToken中
  ```


##### 2.2.3.4 案例

- 请求示例:

  ```json
  {
      "mobile":"17711112222",
      "password":"e10adc3949ba59abbe56e057f20f883e",
      "accountType":1
  }
  ```

- 响应示例:

  ```json
  {
  	"code": 200,
  	"status": "OK",
  	"result": {
  		"roleId": 1,
  		"roleName": "ROLE_MERCHANT"
  	},
  	"message": "登陆成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "message": "用户名或密码错误,还有5次尝试机会"
  }
  ```

---

#### 2.2.4 登出接口

##### 2.2.4.1 接口协议描述

- 接口描述: 判断邀请码是否正确接口 查看邀请码是否正确/被使用
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/auth/logout

##### 2.2.4.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

- 注

  ```java
  无
  ```

##### 2.2.4.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释         | 是否空 |
| ------ | -------- | ---- | ------------ | ------ |
| result | String   |      | 返回接口状态 | 否     |

##### 2.2.4.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "退出成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "登出异常"
  }
  ```

---

### 2.3 首页

---

#### 2.3.1 模板推荐列表接口

##### 2.3.1.1 接口协议描述

- 接口描述: 首页场景模板推荐列表拉取
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/scene/getRecommendTempList

##### 2.3.1.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

- 注

  ```java
  无
  ```

##### 2.3.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                            | 数据类型     | 长度                | 注释                          | 是否空 |
| --------------------------------- | ------------ | ------------------- | ----------------------------- | ------ |
| result                            | List<Object> | v1.0.0数组长度固定2 | 返回推荐模板列表              | 否     |
| result[index].senceTempType       | int          |                     | 场景模板 类型 1-打卡 2-转载   | 否     |
| result[index].senceTempName       | String       |                     | 场景模板 描述   打卡 / 转载 / | 否     |
| result[index].senceTempImgUrl     | String       |                     | 场景模板 缩略图               | 否     |
| result[index].isMade              | int          |                     | 是否已经制作 1-是 2-否        | 否     |
| result[index].senceTempPreviewUrl | String       |                     | 场景模板 预览页面地址         | 否     |

##### 2.3.1.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
  	"code": 200,
  	"status": "OK",
  	"result": [{
  		"senceTempType": 1,
  		"senceTempName": "打卡",
  		"senceTempImgUrl": "http://xx.xx.com/xx/dk.png",
  		"isMade": 1,
  		"senceTempPreviewUrl": "http://xx.xx.com/xx/scene/dk.html"
  	}, {
  		"senceTempType": 2,
  		"senceTempName": "转载",
  		"senceTempImgUrl": "http://xx.xx.com/xx/zz.png",
  		"isMade": 2,
  		"senceTempPreviewUrl": "http://xx.xx.com/xx/scene/zz.html"
  	}],
  	"message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "message": "操作失败"
  }
  ```

---

### 2.4 模板管理

---

#### 2.4.1 我的场景模板列表接口

##### 2.4.1.1 接口协议描述

- 接口描述: 首页-模板管理页-我的模板列表拉取
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/scene/getMySceneList

##### 2.4.1.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

- 注

  ```java
  无
  ```

##### 2.4.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                      | 数据类型     | 长度            | 注释                                                         | 是否空 |
| --------------------------- | ------------ | --------------- | ------------------------------------------------------------ | ------ |
| result                      | List<Object> | 0-2（list长度） | 返回我的模板列表  长度与2.3.1接口返回值List元素中 属性isMade=1 的数量相同 | 否     |
| result[index].id            | int          | 11              | 我的创建的场景 id                                            | 否     |
| result[index].senceTempType | int          |                 | 场景模板 类型 1-打卡 2-转载                                  | 否     |
| result[index].senceTempName | String       |                 | 场景模板 描述  打卡 / 转载 /                                 | 否     |
| result[index].materialUrl   | String       |                 | *我的创建的场景 中企业宣传图的地址*                          | 否     |
| result[index].previewUrl    | String       |                 | 我的创建的场景 预览页面地址                                  | 否     |

##### 2.4.1.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": [{
           "id": 1007
  		"senceTempType": 1,
  		"senceTempName": "打卡",
  		"materialUrl": "http://xx.xx.com/xx/my/dk.png",
  		"previewUrl": "http://xx.xx.com/xx/sence/1/1007"
  	},{
           "id": 1018
  		"senceTempType": 2,
  		"senceTempName": "转载",
  		"materialUrl": "http://xx.xx.com/xx/my/zz.png",
  		"previewUrl": "http://xx.xx.com/xx/sence/2/1018"
  	}],
      "message": "获取成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.4.2  上传图片接口 todo

##### 2.4.2.1 接口协议描述

- 接口描述: 上传图片接口
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/common/uploadImg

##### 2.4.2.2 请求要素说明

| 参数名  | 数据类型 | 长度 | 注释                                        | 可为空 |
| ------- | -------- | ---- | ------------------------------------------- | ------ |
| imgType | int      |      | 图片类型 1-logo图片 2-企业宣传图类 3-素材类 | N      |
| 图片    |          |      |                                             |        |

- 注

  ```java
  邀请码校验正则 : ^\d{6}$
  ```

##### 2.4.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.4.2.4 案例

- 请求示例:

  ```json
  {
      "invitCode":"654321"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "可使用"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "验证码已使用/验证码不正确"
  }
  ```

---

#### 2.4.3 场景模板制作/编辑保存接口

##### 2.4.3.1 接口协议描述

- 接口描述: 打开场景模板制作与更新接口  
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/scene/{senceTempType}/createOrUpdate

##### 2.4.3.2 请求要素说明

| 参数名      | 数据类型 | 长度 | 注释             | 可为空 |
| ----------- | -------- | ---- | ---------------- | ------ |
| id          | int      | 11   | 打卡场景id       | Y      |
| logoUrl     | String   |      | 企业logo图片     | Y      |
| materialUrl | String   |      | 企业宣传图片地址 | N      |

路径参数: 

| 路径参数      | 数据类型 | 长度 | 注释         | 可为空 |
| ------------- | -------- | ---- | ------------ | ------ |
| senceTempType | int      |      | 场景模板类型 | N      |

- 注

  ```java
  {senceTempType}为1(即打卡模板)时 logoUrl 不为空 
  {senceTempType}为2(即转载模板)时 logoUrl 可为空
  id可空  传值则为更新操作
  ```

##### 2.4.3.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.4.3.4 案例

- 请求示例:

  ```json
  {
      "id": 1007,
      "logoUrl": "http://xx.xx.com/xx/logo/xxx.png",
      "materialUrl": "http://xx.xx.com/xx/material/xxx.png"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.4.4 我创建的场景模板查询接口

##### 2.4.4.1 接口协议描述

- 接口描述: 场景模板管理-我的模板-编辑时回显回调
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/scene/{senceTempType}/getById

##### 2.4.4.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释   | 可为空 |
| ------ | -------- | ---- | ------ | ------ |
| id     | int      | 11   | 场景id | N      |

路径参数: 

| 路径参数      | 数据类型 | 长度 | 注释         | 可为空 |
| ------------- | -------- | ---- | ------------ | ------ |
| senceTempType | int      |      | 场景模板类型 | N      |

##### 2.4.4.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名             | 数据类型 | 长度 | 注释           | 是否空 |
| ------------------ | -------- | ---- | -------------- | ------ |
| result             | Object   |      | 打卡模板信息   | 是     |
| result.logoUrl     | String   |      | logo地址       | 是     |
| result.materialUrl | String   |      | 企业宣传度地址 | 是     |

- 注

  ```java
  {senceTempType}为1(即打卡模板)时 logoUrl 不为空 
  {senceTempType}为2(即转载模板)时 logoUrl 为空
  ```

##### 2.4.4.4 案例

- 请求示例:

  ```json
  {
     "id": 1007
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": {
          "id": 1007
          "logoUrl": "http://xx.xx.com/xx/logo/xxx.png",
          "materialUrl": "http://xx.xx.com/xx/material/xxx.png"
  	},
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

#### 2.4.5 检查场景模板是否创建接口

##### 2.4.5.1 接口协议描述

- 接口描述:  用户创建完场景模板后，才可创建活动模板。如用户没有场景模板，点击活动管理页面-“创建活动“需要toast提示用户：请先创建场景模板 业务逻辑需要接口 
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/scene/checkCreated

##### 2.4.5.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

##### 2.4.5.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释                       | 是否空 |
| ------ | -------- | ---- | -------------------------- | ------ |
| result | boolean  |      | 返回用户是否创建了场景模板 | 否     |

##### 2.4.5.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": true,
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

### 2.5  活动管理

---

#### 2.5.1 活动模板列表查询接口

##### 2.5.1.1 接口协议描述

- 接口描述: 活动模板列表查询接口  v1.0.0 活动模板只有一个
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/act/getTempList

##### 2.5.1.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

- 注

  ```java
  无
  ```

##### 2.5.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                          | 数据类型     | 长度 | 注释                                    | 是否空 |
| ------------------------------- | ------------ | ---- | --------------------------------------- | ------ |
| result                          | List<Object> |      | 返回活动模板列表  v1.0.0 长度为1        | 是     |
| result[index].actTempType       | int          |      | 活动模板 类型 例: 1  v1.0.0活动模板单个 |        |
| result[index].actTempName       | String       |      | 活动模板 名称   例: 在线购买            |        |
| result[index].actTempImgUrl     | String       |      | 活动模板 缩略图                         |        |
| result[index].actTempPreviewUrl | String       |      | 活动模板 预览地址                       |        |

##### 2.5.1.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": [{
          "actTempType": 1,
          "actTempName": "在线购买",
          "actTempImgUrl": "http://xx.xx.com/xx/act/zxgm.png",
          "actTempPreviewUrl": "http://xx.xx.com/xx/act/zxgm.html"
      }],
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.5.2 我的活动模板列表接口

##### 2.5.2.1 接口协议描述

- 接口描述: 活动管理 我创建的活动列表    分页模式    时间倒叙
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/act/getMyTempList

##### 2.5.2.2 请求要素说明

| 参数名   | 数据类型 | 长度 | 注释                        | 可为空 |
| -------- | -------- | ---- | --------------------------- | ------ |
| pageSize | int      |      | 页面大小  可空  默认 10     | Y      |
| pageNum  | int      |      | 第几页 从1开始  可空 默认 1 | Y      |

- 注

  ```java
  无
  ```

##### 2.5.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                    | 数据类型     | 长度 | 注释                                   | 是否空 |
| ------------------------- | ------------ | ---- | -------------------------------------- | ------ |
| result                    | List<Object> |      | 返回我创建的活动列表                   | 否     |
| result[index].id          | int          |      | 我创建的活动 id                        |        |
| result[index].actTempType | String       |      | 活动模板 类型                          |        |
| result[index].actTempName | String       |      | 活动模板类型 名称                      |        |
| result[index].title       | String       |      | 我创建的活动 名称                      |        |
| result[index].materialUrl | String       |      | 我创建的活动 物料图片地址              |        |
| result[index].actStatus   | int          |      | 我创建的活动 状态 1已发布 0草稿/未发布 |        |
| result[index].previewUrl  | String       |      | 我创建的活动 预览地址                  |        |

##### 2.5.2.4 案例

- 请求示例:

  ```json
  {
      "pageSize": 10,
      "pageNum": 1
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": [
          {
              "id": 2006,
              "actTempType": 1,
              "actTempName": "在线购买",
              "title": "北京兴趣班学童...",
              "materialUrl": "http://xx.xx.com/xx/act/my/zxgm.png",
              "actStatus": 1,
              "previewUrl": "http://xx.xx.com/xx/act/1/2006"
      	},{
              "id": 2008,
              "actTempType": 1,
              "actTempName": "在线购买",
              "title": "北京兴趣班学童...",
              "materialUrl": "http://xx.xx.com/xx/act/my/zxgm.png",
              "actStatus": 2,
              "previewUrl": "http://xx.xx.com/xx/act/1/2008"
      	},
            ....
                ],
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.5.3  活动模板制作/编辑接口

##### 2.5.3.1 接口协议描述

- 接口描述: 判断邀请码是否正确接口 查看邀请码是否正确/被使用  制作前首先校验用户是否创建了场景模板 

  ​	参照: [2.4.5 检查场景模板是否创建接口](#2.4.5 检查场景模板是否创建接口)

- 接口协议: HTTP POST

- 数据格式: JSON

- 接口调用方向: MA -> AS

- URL Mapping: http://${ip}:${port}/${basePath}/act/{actTempType}/createOrUpdate

##### 2.5.3.2 请求要素说明

| 参数名         | 数据类型 | 长度 | 注释               | 可为空 |
| -------------- | -------- | ---- | ------------------ | ------ |
| id             | int      | 11   | 活动id             | Y      |
| materialUrl    | String   |      | 活动宣传图         | N      |
| price          | int      |      | 价格 转换成 分传递 | N      |
| endTime        | String   |      | 活动截止日期       | N      |
| contactNumber  | String   |      | 商户电话           | N      |
| contactAddress | String   | 0-40 | 商户地址           | N      |

* 注

  ```java
  id 在编辑保存操作时传入 会进行更新操作 已发布活动不能更新
  price  保留两位小数  与服务端传递 将其转化为单位分
  日期格式为 yyyy-MM-dd
  ```

* endTime 从当天到60天后  时间若需要校准 参照接口:  [2.1.2 时间校准接口](#2.1.2 时间校准接口)

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.5.3.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释     | 是否空 |
| ------ | -------- | ---- | -------- | ------ |
| result | String   |      | 返回状态 | 否     |

##### 2.5.3.4 案例

- 请求示例:

  ```json
  {
      "id": "4008",
      "materialUrl": "http://xxx",
      "price": 2998,
      "endTime": "2019-08-08",
      "contactNumber": "4006660909",
      "contactAddress": "北京市海淀区宝盛南路1号院16号楼"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.5.4 删除我的活动接口

##### 2.5.4.1 接口协议描述

- 接口描述:  删除
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/act/{actTempType}/deleteById

##### 2.5.4.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释   | 可为空 |
| ------ | -------- | ---- | ------ | ------ |
| id     | int      | 11   | 活动id | N      |

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.5.4.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.5.4.4 案例

- 请求示例:

  ```json
  {
      "id": 4008
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.5.5 我的活动生成接口

##### 2.5.5.1 接口协议描述

- 接口描述: 将已保存未生成的活动转变为生成状态
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/art/{actTempType}/releaseById

##### 2.5.5.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释   | 可为空 |
| ------ | -------- | ---- | ------ | ------ |
| id     | int      | 11   | 活动id | N      |

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.5.5.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释         | 是否空 |
| ------ | -------- | ---- | ------------ | ------ |
| result | String   |      | 返回操作状态 | 否     |

##### 2.5.5.4 案例

- 请求示例:

  ```json
  {
      "id":4008
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.5.6 我的活动信息查询接口

##### 2.5.6.1 接口协议描述

- 接口描述:  编辑活动时  我创建的活动数据回显   未生成的活动调用 用于编辑保存
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/art/{actTempType}/getById

##### 2.5.6.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释   | 可为空 |
| ------ | -------- | ---- | ------ | ------ |
| id     | int      | 11   | 活动id | N      |

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.5.6.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                | 数据类型 | 长度 | 注释           | 是否空 |
| --------------------- | -------- | ---- | -------------- | ------ |
| result                | Object   |      | 返回活动的信息 | 否     |
| result.materialUrl    | String   |      | 活动宣传图     |        |
| result.price          | int      |      | 价格  单位 分  |        |
| result.endTime        | String   |      | 活动截止日期   |        |
| result.contactNumber  | String   |      | 商户电话       |        |
| result.contactAddress | String   |      | 商户地址       |        |

##### 2.5.6.4 案例

- 请求示例:

  ```json
  {
      "id":4008
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": {
          "materialUrl": "http://xxx",
          "price": 2998,
          "endTime": "2019-08-08",
          "contactNumber": "4006660909",
          "contactAddress": "北京市海淀区宝盛南路1号院16号楼"
      },
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

------

### 2.6 学员管理

---

#### 2.6.1 学员列表查询接口

##### 2.6.1.1 接口协议描述

- 接口描述: 学员列表 模糊查询 按名称排序
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/stu/getListByName

##### 2.6.1.2 请求要素说明

| 参数名     | 数据类型 | 长度 | 注释                    | 可为空 |
| ---------- | -------- | ---- | ----------------------- | ------ |
| searchName | String   | 0-20 | 查询名称                | Y      |
| pageSize   | int      |      | 页面大小  可空  默认 10 | Y      |
| pageNum    | int      |      | 第几页  可空  默认 1    | Y      |

##### 2.6.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名                   | 数据类型     | 长度  | 注释         | 是否空 |
| ------------------------ | ------------ | ----- | ------------ | ------ |
| result                   | List<Object> |       | 学员列表集合 | 是     |
| result[index].id         | int          | 11    | 学员id       | 否     |
| result[index].name       | String       |       | 学员姓名     | 否     |
| result[index].headimgurl | String       | 0-200 | 学员头像     | 是     |

##### 2.6.1.4 案例

- 请求示例:

  ```json
  {
      "searchName": "张三",
      "pageSize": 30,
      "pageNum": 1
  }
  ```

- 响应示例:

  ```json
  {
  	"code": 200,
  	"status": "OK",
  	"result": [{
  		"id": 701,
  		"name": "张白",
  		"headimgurl": "http://wx.qlogo.cn/mmopen/PiajxSqBRaEL7hBAAkJ50Dp9k3d1ID1xufMw2..."
  	}, {
  		"id": 703,
  		"name": "张大",
  		"headimgurl": "http://wx.qlogo.cn/mmopen/PiajxSqBRaEL7hBAAkJ50Dp9k3d1ID1xufMw2..."
  	},
                 ...
                ],
  	"message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.6.2 学员信息查询接口

##### 2.6.2.1 接口协议描述

- 接口描述: 学员列表 点击单条目跳转至学员信息详细 数据回显拉取接口
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/stu/getById

##### 2.6.2.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释   | 可为空 |
| ------ | -------- | ---- | ------ | ------ |
| id     | int      | 11   | 学员id | N      |

##### 2.6.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名          | 数据类型 | 长度 | 注释                | 是否空 |
| --------------- | -------- | ---- | ------------------- | ------ |
| result          | Object   |      | 学员信息对象        | 否     |
| result.number   | String   |      | 学员 编号           |        |
| result.nickname | String   |      | 学员 微信昵称       |        |
| result.name     | String   |      | 学员 姓名           |        |
| result.petName  | String   |      | 学员 小名/英文名    |        |
| result.bornDate | String   |      | 出生日期 yyyy.MM.dd |        |
| result.mobile   | String   | 11   | 手机号码            |        |
| result.remark   | String   | 0-40 | 备注                |        |

* 注:

  ```java 
  学员编号 为 企业ID+5位数字，5位数字从00001开始累计，针对每个商户用户都从1开始计算
  ```

##### 2.6.2.4 案例

- 请求示例:

  ```json
  {
      "id": 701
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": {
      	"number": "1000700003",
          "nickname": "zbb 0-0",
          "name": "张白白",
          "petName": "豆豆",
          "bornDate": "2000.05.03",
          "mobile": "17700001234",
          "remark": "店长A备注:这个学员的朋友中有潜在客户"
      },
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

#### 2.6.3 学员备注更新接口

##### 2.6.3.1 接口协议描述

- 接口描述: 学员备注更改后 调用服务端保存接口
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/stu/updateRemarkById

##### 2.6.3.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释                | 可为空 |
| ------ | -------- | ---- | ------------------- | ------ |
| id     | int      | 11   | 学员id              | N      |
| remark | String   | 0-40 | 学员备注 四十字以内 | Y      |

- 注

  ```java
  注意此处是学员的id 而不是学员编码
  ```

##### 2.6.3.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.6.3.4 案例

- 请求示例:

  ```json
  {
      "id": 701,
      "remark": "店长A备注:这个学员的朋友中有潜在客户 有电话了.."
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

### 2.7 企业中心

---

#### 2.7.1 拉取登录信息接口

##### 2.7.1.1 接口协议描述

- 接口描述: 企业中心 拉取登录用户 及企业数据 接口
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/auth/getLoginInfo

##### 2.7.1.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

##### 2.7.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名     | 数据类型 | 长度 | 注释             | 是否空 |
| ---------- | -------- | ---- | ---------------- | ------ |
| result     | Object   |      | 返回当前登录信息 | 否     |
| headimgurl | String   |      | 头像地址         | 否     |
| orgId      | int      | 11   | 企业id           | 否     |
| mobile     | String   | 11   | 手机号码         | 否     |

##### 2.7.1.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": {
          "headimgurl": "http://wx.asaaa.png",
          "orgId": 10005,
          "mobile": "17700001234"
      },
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.7.2 更换头像接口

##### 2.7.2.1 接口协议描述

- 接口描述:  用户更换自己的头像
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/auth/changeHeadimgurl

##### 2.7.2.2 请求要素说明

| 参数名     | 数据类型 | 长度  | 注释         | 可为空 |
| ---------- | -------- | ----- | ------------ | ------ |
| headimgurl | String   | 0-200 | 新的头像地址 | N      |

##### 2.7.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释           | 是否空 |
| ------ | -------- | ---- | -------------- | ------ |
| result | String   |      | 返回邀请码状态 | 否     |

##### 2.7.2.4 案例

- 请求示例:

  ```json
  {
      "headimgurl":"http://xxxxxxxxxx"
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": "success",
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.7.3 检查更新接口  todo

---

### 2.8  作品模块

---

#### 2.8.1 获取对应场景 已创建的场景模板的模板ID接口 

##### 2.8.1.1 接口协议描述

- 接口描述: 获取对应场景下的用户创建的场景模板id   用于判断用户是否创建了对应模板 以及创建作品时的数据提交
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/sence/{senceTempType}/getId

##### 2.8.1.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

路径参数: 

| 路径参数      | 数据类型 | 长度 | 注释         | 可为空 |
| ------------- | -------- | ---- | ------------ | ------ |
| senceTempType | int      |      | 场景模板类型 | N      |

##### 2.8.1.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释               | 是否空 |
| ------ | -------- | ---- | ------------------ | ------ |
| result | int      |      | 返回我创建的模板id | 否     |

* 注: 

  ```java
  如果返回0则表示没有创建 提示并引导用户去创建对应场景模板
  ```

##### 2.8.1.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": 1007/0,
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.8.2 获取已创建的在线购买活动模板的模板ID接口

##### 2.8.2.1 接口协议描述

- 接口描述: 检测用户是否有可使用(已生成的)活动模板  
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/act/{actTempType}/checkCreated

##### 2.8.2.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.8.2.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名 | 数据类型 | 长度 | 注释                                | 是否空 |
| ------ | -------- | ---- | ----------------------------------- | ------ |
| result | boolean  |      | 返回是否用户创建生成了活动 状态标识 | 否     |

* 注 

  ```java
  如果返回false则表示没有可用活动 提示并引导用户去创建生成活动
  ```

##### 2.8.2.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": true,
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

---

#### 2.8.3 已生成活动列表拉取

##### 2.8.3.1 接口协议描述

- 接口描述: 作品创建页面  活动下拉框
- 接口协议: HTTP POST
- 数据格式: JSON
- 接口调用方向: MA -> AS
- URL Mapping: http://${ip}:${port}/${basePath}/act/{actTempType}/getMyList

##### 2.8.3.2 请求要素说明

| 参数名 | 数据类型 | 长度 | 注释 | 可为空 |
| ------ | -------- | ---- | ---- | ------ |
| 无参数 |          |      |      |        |

路径参数: 

| 路径参数    | 数据类型 | 长度 | 注释                      | 可为空 |
| ----------- | -------- | ---- | ------------------------- | ------ |
| actTempType | int      |      | 活动模板类型 v1.0.0 默认1 | N      |

##### 2.8.3.3 响应要素说明

基本响应包体参照: [1.5 统一返回值数据模型](#1.5 统一返回值数据模型)

code 返回200时:

| 参数名              | 数据类型     | 长度 | 注释               | 是否空 |
| ------------------- | ------------ | ---- | ------------------ | ------ |
| result              | List<Object> | 1-n  | 返回可使用活动列表 | 否     |
| result[index].id    | int          | 11   | 活动id             | 否     |
| result[index].title | String       |      | 活动名称           | 否     |

##### 2.8.3.4 案例

- 请求示例:

  ```json
  {
  }
  ```

- 响应示例:

  ```json
  {
      "code": 200,
      "status": "OK",
      "result": [{
          "id": 2006 ,
          "title": "北京兴趣班学童xx..."
      },{
          "id": 2008 ,
          "title": "北京兴趣班学童xx..."
      },
                ...
                ],
      "message": "操作成功"
  }
  ```

- 失败请求示例

  ```json
  {
      "code": 400,
      "status": "Bad Request",
      "result": "fail",
      "message": "操作失败"
  }
  ```

#### 2.8.4 完成并发布到家长端/保存作品接口





2.8.5 













