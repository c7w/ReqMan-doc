# 项目管理接口

## 提示和说明
!!! info "成员属性"
    - supermaster：项目管理员
    - dev：开发工程师
    - qa：质保工程师
    - sys：系统工程师
    - member：项目成员

## 创建项目

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


