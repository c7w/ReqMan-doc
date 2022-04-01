<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# 项目管理接口

## 提示和说明
!!! info "成员属性"
    - supermaster：项目管理员
    - dev：开发工程师
    - qa：质保工程师
    - sys：系统工程师
    - member：项目成员

## 基本操作

### 创建项目
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/create_project/</span>
</div>

用于创建新项目

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "title": "雷克曼",
            "description": "ReqMan"
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
|title|str|项目名称|
|description|str|项目描述|
|title|str|项目头像（可选）|

#### 响应状态
|状态码|说明|
|-|-|
|0|用户名可用|
|1|创建失败，原因是项目名太长|

### 显示项目信息
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/project/</span>
</div>

用于项目成员查看项目信息。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "project": {
                    "id": 1,
                    "title": "雷克曼",
                    "description": "ReqMan",
                    "createdAt": 1648278867.751802,
                    "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q=="
                },
                "users": [
                    {
                        "id": 1,
                        "name": "c7w",
                        "email": "admin@cc7w.cf",
                        "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q==",
                        "createdAt": 1648050909.599101,
                        "email_verified": false,
                        "role": "supermaster"
                    }
                ]
            }
        }
        ```

#### 请求权限 
项目任意成员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|title|str|项目名称|
|description|str|项目描述|
|title|str|项目头像（可选）|

#### 响应状态
|状态码|说明|
|-|-|
|0|获取项目信息成功|

!!! bug
    中间某个版本实现时误将avatar放到了data的下面而不是project的下面，后续考虑移除。（示例响应已经移除，但目前dev分支在两个地方都加了项目avatar）

## 成员管理

### 添加用户
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/project_add_user/</span>
</div>

用于项目管理员给项目添加成员用户。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "user": 12,
            "role": "dev"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|user|int/str|要添加用户的ID|
|role|str|成员属性|

#### 响应状态
|状态码|说明|
|-|-|
|0|添加成功|
|1|添加失败，原因可能是用户已经在项目中或成员属性无效|
|2|用户ID无效|
|3|项目ID无效|

!!! todo 
    code = 2，3尚未实现




### 删除用户

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/project_rm_user/</span>
</div>

用于项目管理员从项目中删除用户。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "user": 12,
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目管理员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|user|int/str|要删除用户的ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|删除成功|
|1|删除失败，原因是(用户, 项目)无效|

### 修改用户角色

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/modify_user_role/</span>
</div>

用于项目管理员修改用户权限。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "role": "sys"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|user|int/str|要添加用户的ID|
|role|str|成员属性|

#### 响应状态
|状态码|说明|
|-|-|
|0|修改成功|
|1|修改失败，原因是(用户, 项目)无效或成员属性无效|


### 获得邀请码
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/get_invitation/</span>
</div>

用于项目管理员获取邀请码。如果已经存在邀请码则返回邀请码，如果不存在邀请码则创建一个新的邀请码。用邀请码加入项目后的默认属性配置于`DEFAULT_INVITED_ROLE`。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1
        }
        ```
    === "响应"
        ```json
        {
            "code": 0，
            "data": {
                "invitation": "AU4YNT7Q"
            }
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|获取邀请码成功|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|invitation|str|项目当前的邀请码|



### 刷新邀请码
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/refresh_invitation/</span>
</div>

用于项目管理员更新邀请码。如果已经存在邀请码，那么已经存在的邀请码将失效，如果不存在邀请码则创建一个新的邀请码。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1
        }
        ```
    === "响应"
        ```json
        {
            "code": 0，
            "data": {
                "invitation": "AU4YNT7Q"
            }
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|获取邀请码成功|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|invitation|str|项目当前的邀请码|


## 项目信息配置

### 上传项目图标
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/upload_project_avatar/</span>
</div>

用于项目管理员上传项目图标。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ ... J7E2Ko9o4Ems3p//2Q=="
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|avatar|str|base64编码，表示项目图标|

#### 响应状态
|状态码|说明|
|-|-|
|0|上传成功|

### 修改项目信息

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/modify_project/</span>
</div>

用于项目管理员修改项目信息

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "title": "雷克曼",
            "description": "ReqMan"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|title|str|项目名称|
|description|str|项目描述|

#### 响应状态
|状态码|说明|
|-|-|
|0|修改成功