
<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# 用户账户接口

!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

## 用户注册 
### 注册接口

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/register/</span>
</div>

用于注册新用户。

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

### 检查用户名是否可用
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/check_username_available/</span>
</div>

用于注册过程中，检查用户名是否可用。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "name": "lambda_x",
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
无
#### 请求参数
|参数|类型|说明|
|-|-|-|
|name|str|要检查占用的用户名|

#### 响应状态
|状态码|说明|
|-|-|
|0|用户名可用|
|1|用户名被占用|


### 检查邮箱是否可用
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/check_email_available/</span>
</div>

用于注册过程中，检查邮箱是否可用。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "email": "joshua@btlmd.com",
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
无
#### 请求参数
|参数|类型|说明|
|-|-|-|
|email|str|要检查占用的邮箱|

#### 响应状态
|状态码|说明|
|-|-|
|0|邮箱可用|
|1|邮箱被占用|

## 登录登出

### 用户登录
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/login/</span>
</div>

用于用户登录。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "identity": "lambda_x",
            "password": "99b1ff8f11781541f7f89f9bd41c4a17"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
无

<!-- 将内容改为permission decorator实现？不改了，因为要变状态码，有风险 -->
#### 请求参数
|参数|类型|说明|
|-|-|-|
|identity|str|用户邮箱或用户名|
|password|str|哈希后的用户密码|

#### 响应状态
|状态码|说明|
|-|-|
|0|登录成功|
|1|登录失败，因为已经登录|
|2|登录失败，因为用户身份不存在|
|3|登录失败，因为用户身份存在，但密码错误|

!!! info "为什么允许用户知道自己密码错误"
    允许用户知道自己密码错误/身份不存在会增加安全隐患。确实。但考虑到我们注册接口会检查用户名及邮箱是否重复，而当前的代理配置使我们无法获知用户IP从而实施流量控制，我们在注册接口事实上已经暴露了用户名/邮箱情况。

    因此，允许用户知道自己密码错误/身份不存在不会增加**额外**的安全隐患。

### 用户登出
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/logout/</span>
</div>

用户用户登出。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
无
#### 请求参数
无
#### 响应状态
|状态码|说明|
|-|-|
|0|登出成功|
|1|登出失败，因为尚未登录|

## 用户信息

### 获取用户信息
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /ums/user/</span>
</div>
用户获取用户信息，包括
- UMS中的账户信息，如用户名，邮箱，头像等等
- RMS中的任务信息，如计划表。

??? example "示例"
    === "请求"
        ```bash 
        curl https://example.com/ums/user/?sessionId=Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "user": {
                    "id": 1,
                    "name": "c7w",
                    "email": "admin@cc7w.cf",
                    "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q==",
                    "createdAt": 1648050909.599101,
                    "email_verified": false
                },
                "projects": [
                    {
                        "id": 1,
                        "title": "雷克曼",
                        "description": "ReqMan",
                        "createdAt": 1648278867.751802,
                        "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q==",
                        "role": "supermaster"
                    }
                ]
            }
        }
        ```

#### 请求权限 
已登录用户
#### 请求参数
无

#### 响应状态
|状态码|说明|
|-|-|
|0|获取用户信息成功|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|user|object|用户信息|
|projects|array|用户对应的项目信息|



## 通过邮箱重置密码
如果用户忘记密码，可以通过已经验证的邮箱来重置密码。
### 重置密码请求接口
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/email_modify_password_request/</span>
</div>

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

#### 请求参数

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
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/email_modify_password_request/</span>
</div>


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
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/email_request/</span>
</div>


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
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/email_verify_callback/</span>
</div>

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
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/user_join_project_invitation/</span>
</div>

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "invitation": "8FNE6SL1",
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
已登录用户
#### 请求参数
|参数|类型|说明|
|-|-|-|
|invitation|str|项目的邀请码|

#### 响应状态
|状态码|说明|
|-|-|
|0|加入成功|
|1|已经在项目中|
|2|邀请码无效|

### 修改头像
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/upload_user_avatar/</span>
</div>
用于用户上传头像。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q==",
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
已登录用户
#### 请求参数
|参数|类型|说明|
|-|-|-|
|avatar|str|base64编码，表示用户的头像|

#### 响应状态
|状态码|说明|
|-|-|
|0|上传成功|

### 修改密码
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/modify_password/</span>
</div>

用于在已知密码的情况下修改用户密码。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "prev": "99b1ff8f11781541f7f89f9bd41c4a17",
            "curr:: "541f1ff9f9bd418f11787f8c4a2180b1"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
已登录用户
#### 请求参数
|参数|类型|说明|
|-|-|-|
|prev|str|旧密码哈希值|
|curr|str|新密码哈希值|

#### 响应状态
|状态码|说明|
|-|-|
|0|修改成功|
|2|修改失败，原因是旧密码不正确。|

### 设置远程用户名
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/set_remote_username/</span>
</div>

用于配置本地用户在指定远程仓库中的用户名。

??? example "示例"
    === "请求"
        ```json
        {
            "url": "gitlab.secoder.net",
            "remote_name": "username",
            "sessionId": "f4wIl1q03HntE5Mr36MO4WeqfMITWH4Q"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目任意成员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|remote_name|str|用户在远程仓库中的用户名|
|url|str/int|远程URL|
|project|str/int|仓库所在项目id|

#### 响应状态
|状态码|说明|
|-|-|
|0|设置成功|
|1|设置失败，因为用户名长于255字符|
|2|设置失败，因为项目中没有这个仓库|