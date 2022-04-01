
<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# 用户账户接口
!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。
    如果你发现请求的状态码为负数，请参见[权限约定](index.md##权限约定)
## 用户注册 
### 注册接口
[POST] `/ums/register/`

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "name": "lambda_x",
            "password": "99b1ff8f11781541f7f89f9bd41c4a17",
            "email": "joshua@btlmd.com"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求参数
|参数|类型|说明|
|-|-|-|
|name|str|用户名|
|password|str|hash后用户密码|
|email|str|用户邮箱|
|invitation|str|项目邀请码（可选）|

#### 响应状态
|状态码|说明|
|-|-|
|0|注册成功|
|1|失败，原因包括：用户已经登录，姓名不合法/已存在，邮箱不合法/已存在|
|2|失败，因为写了邀请码，但邀请码无效|


## 登录登出

### 用户登录

### 用户登出

## 用户信息

### 获取用户信息



## 通过邮箱重置密码
如果用户忘记密码，可以通过已经验证的邮箱来重置密码。
### 重置密码请求接口

 [POST] `/ums/email_modify_password_request/`

此接口用于请求发送一封用于修改密码的邮件。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "joshua@btlmd.com"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求

|参数|类型|说明|
|-|-|-|
|email|str|用户的主邮箱|

#### 响应状态

|状态码|说明|
|-|-|
|0|邮件正确，发送成功|
|1|邮件服务暂时不可用|
|2|不存在这个主邮件|
|3|主邮箱没有验证|
|4|服务器忙|
2,3,4 尚未实现

### 重置密码回调接口

[POST] `/ums/email_modify_password_request/`

此接口用于利用发送给用户的哈希值来重置密码，包括两个阶段。
``` mermaid
sequenceDiagram
  autonumber
  Frontend->>Backend: 请求1
  Note right of Backend: 验证hash1
  Backend-->>Frontend: 响应1：hash2
  Note left of Frontend: 用户修改密码
  Frontend->>Backend: 请求2
  Note right of Backend: 验证hash1, hash2
  Backend-->>Frontend: 响应2
```

??? example "示例"
    === "请求1"
        ```json
        {
            "sessionId": "325324gw3",
            "stage": 1,
            "hash1": "9388a349cf5dd30f893fee997647ae9f5ca62cca5232e0d5e7a9dbdf7af513a4"
        }
        ```
    === "响应1"
        ```json
        {
            "code": 0,
            "data": {
                "hash2": "66d370412e76a9fe419132df190400479547dfb1ca32094cb960b4ff3c801de7"
            }
        }
        ```
    === "请求2"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "stage": 2,
            "hash1": "9388a349cf5dd30f893fee997647ae9f5ca62cca5232e0d5e7a9dbdf7af513a4",
            "hash2": "66d370412e76a9fe419132df190400479547dfb1ca32094cb960b4ff3c801de7",
            "password": "newpassword"
        }
        ```
    === "响应2"
        ```json
        {
            "code": 0
        }
        ```

#### 请求1参数

|参数|类型|说明|
|-|-|-|
|stage|int/str|值为1|
|hash1|str|邮件中的hash1值|

#### 响应1状态

|状态码|说明|
|-|-|
|0|成功|
|1|hash1无效或已经使用|
|2|hash1过期，配置于`EMAIL_EXPIRE_SECONDS`|

#### 响应1数据

|属性|类型|说明|
|-|-|-|
|hash2|int|用于修改密码的hash2|

#### 请求2参数
|参数|类型|说明|
|-|-|-|
|stage|int/str|值为2|
|hash1|str|邮件中的hash1值|
|hash2|str|响应1中给出的hash2|
|password|str|哈希后的新密码|

#### 响应2状态

|状态码|说明|
|-|-|
|0|成功|
|1|(hash1, hash2)偶对无效或已经使用|
|2|hash2过期，配置于`RESETTING_STATUS_EXPIRE_SECONDS`|





## 邮箱配置与验证
### 邮箱操作请求接口

[POST] `/ums/email_request/`

此接口用于请求添加/修改/删除/验证副邮箱，或修改/验证主邮箱。对主邮箱的添加/删除操作将会抛出`Param Err`，导致返回 `-1` 。
!!! info "状态码好多？"
    不同操作组合间的状态码含义是基本相同的。

#### 修改邮箱

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "joshua@btlmd.com"，
            "op": "modify",
            "type": "major"

        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

##### 请求参数
|参数|类型|说明|
|-|-|-|
|email|str|新邮箱|
|previous|str|要被修改的副邮箱，修改副邮箱时提供，修改主邮箱无需提供|
|op|str|"modify"，表示修改邮箱|
|type|str|"major"/"minor"，表示主/副邮箱|

##### 响应状态
|状态码|说明|
|-|-|
|0|修改成功，已经向邮箱发送了验证邮件|
|1|修改成功，但没有发送验证邮件，因为邮件服务不可用|
|2|修改成功，但没有发送验证邮件，因为用户操作过于频繁，该用户申请发送邮件的间隔小于 `EMAIL_MIN_INTERVAL`|
|3|修改失败，因为副邮箱副邮箱和主邮箱相同|
|4|修改失败，因为已经存在同类型的邮箱|
|5|修改失败，因为previous所表示的副邮箱不存在|
|6|修改失败，因为所提供的邮箱至少有一个是非法的|

#### 验证邮箱

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "joshua@btlmd.com"，
            "op": "verify",
            "type": "major"

        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

##### 请求参数
|参数|类型|说明|
|-|-|-|
|email|str|请求验证的邮箱|
|op|str|"verify"，表示修改邮箱|
|type|str|"major"/"minor"，表示主/副邮箱|

##### 响应状态
|状态码|说明|
|-|-|
|0|已经向邮箱发送了验证邮件|
|1|没有发送验证邮件，因为邮件服务不可用|
|2|没有发送验证邮件，因为用户操作过于频繁，该用户申请发送邮件的间隔小于 `EMAIL_MIN_INTERVAL`|
|6|邮箱是非法的|
|7|邮箱已经验证|

#### 删除副邮箱

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "tangentnightydegree@foxmail.com"，
            "op": "rm",
            "type": "minor"

        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

##### 请求参数
|参数|类型|说明|
|-|-|-|
|email|str|要删除的邮箱|
|op|str|"rm"，表示删除邮箱|
|type|str|"minor"，表示副邮箱|

##### 响应状态
|状态码|说明|
|-|-|
|0|删除成功|
|5|删除失败，因为email所表示的副邮箱不存在|
|6|删除失败，因为email所表示的副邮箱是非法的|

#### 添加副邮箱

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "tangentnightydegree@foxmail.com"，
            "op": "add",
            "type": "minor"

        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```
        
##### 请求参数
|参数|类型|说明|
|-|-|-|
|email|str|要添加的邮箱|
|op|str|"add"，表示添加邮箱|
|type|str|"minor"，表示副邮箱|

##### 响应状态
|状态码|说明|
|-|-|
|0|添加成功，已经向邮箱发送了验证邮件|
|1|添加成功，但没有发送验证邮件，因为邮件服务不可用|
|2|添加成功，但没有发送验证邮件，因为用户操作过于频繁，该用户申请发送邮件的间隔小于 `EMAIL_MIN_INTERVAL`|
|3|添加失败，因为副邮箱副邮箱和主邮箱相同|
|4|添加失败，因为已经存在同类型的邮箱|
|6|添加失败，因为email所表示的副邮箱是非法的|

### 邮箱验证回调接口

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "hash": "c5f2528c8701d4643f1118da7ae8d20701fecddc6997106e49e018e1996f6928"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```
        
#### 请求参数
|参数|类型|说明|
|-|-|-|
|hash|str|发送到邮箱中的hash值|

#### 响应状态
|状态码|说明|
|-|-|
|0|验证成功|
|1|hash无效|
|2|hash过期|
|3|该邮箱和数据库冲突|

!!! info "为什么hash值可能和数据库冲突"
    例如，用户在该邮箱尚未验证前把邮箱给删除了。
    
## 用户信息配置

### 通过邀请码加入项目

### 上次头像

### 修改密码