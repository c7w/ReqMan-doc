<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>
# RDTS 基础模块接口

!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

!!! note "统一接口"
    有关 RMS 中模型的接口全部通过以下接口进行访问和操作，其中，查询操作通过 GET 请求，而创建、修改以及删除操作通过 POST 请求。

<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project/</span>
</div>

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/project/</span>
</div>

## 操作 Repository

### 查询 Repository

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'repo'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的仓库列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"repo"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'url': '', 
                'project': 1, 
                'title': 'repo1', 
                'description': 'a common repo', 
                'createdAt': 1650883913.901973, 
                'createdBy': 1, 
                'disabled': False, 
                'remote': None
            }]
        }
        ```

### 创建 Repository
### 修改 Repository
!!! info "更新提示"
    考虑到爬虫中使用的远程仓库模型，对创建和修改Repository的接口进行了重构，现移至了[创建和修改远程仓库](remote.md#_3)
<!--
#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'repo'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|仓库的标题，最大长度255|
|url|str|仓库的链接，最大长度 255|
|description|str|仓库的描述|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"repo"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "a",
                    "description": "sx",
                    "url": "233.edu"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 修改 Repository

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'repo'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的仓库的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|仓库的标题，最大长度255，可选|
|url|str|仓库的链接，最大长度 255，可选|
|project|int|仓库所在项目的 id，可选|
|description|str|仓库的描述，可选|
|createdBy|int|创建者的 id，可选|
|createdAt|float|创建的时间戳，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"repo"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aa",
                    "description": "bb",
                    "createdAt":"123456789.0"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```
-->
### 删除 Repository

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'repo'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的仓库的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"repo"，
            "operation":"delete",
            "data":{
                 "id":1
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 Commit

### 查询 Commit

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'commit'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的提交记录列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"commit"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'hash_id': 'acbd', 
                'repo': 1, 'title': 
                'Test.001.001', 
                'message': 'just a test', 
                'commiter_email': 'admin@233.edu', 
                'commiter_name': '233', 
                'createdAt': 1.0, 
                'url': '233.edu', 
                'disabled': False, 
                'user_committer': None, 
                'additions': -1, 
                'deletions': -1},
            {
                'id': 2, 
                'hash_id': 'ToUseID', 
                'repo': 1, 
                'title': 'Test.001.001', 
                'message': 'just a test', 
                'commiter_email': 'admin@233.edu', 
                'commiter_name': '233', 
                'createdAt': 1.0, 
                'url': '233.edu', 
                'disabled': False, 
                'user_committer': None, 
                'additions': -1, 
                'deletions': -1
            }]
        }
        ```

### 创建 Commit

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'commit'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255|
|url|str|提交记录的链接|
|repo|int|提交记录所在仓库的 id|
|hash_id|str|提交记录的 hash_id，最大长度 255|
|message|str|提交记录的描述信息|
|commiter_email|str|提交者的邮箱，最大长度 255|
|commiter_name|str|提交者的名称，最大长度 255|
|createdAt|float|创建时间|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"commit",
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "a",
                    "url": "233.edu",
                    "repo": 2,
                    "hash_id":"acs",
                    "message":"Some info",
                    "commiter_email":"admin@233.edu",
                    "commiter_name":"2333",
                    "createdAt":"123.0"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 修改 Commit

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'commit'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的仓库的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255，可选|
|url|str|提交记录的链接，可选|
|repo|int|提交记录所在仓库的 id，可选|
|hash_id|str|提交记录的 hash_id，最大长度 255，可选|
|message|str|提交记录的描述信息，可选|
|commiter_email|str|提交者的邮箱，最大长度 255，可选|
|commiter_name|str|提交者的名称，最大长度 255，可选|
|createdAt|float|创建时间，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"commit"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aa",
                    "createdAt":"123456789.0"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 Commit

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'commit'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的提交记录的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"commit"，
            "operation":"delete",
            "data":{
                 "id":1
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 MergeRequest

### 查询 MergeRequest

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'mr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的合并请求列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"mr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'merge_id': 1, 
                'repo': 1, 
                'title': 'Merge', 
                'description': '98',
                'state': 'opened', 
                'authoredByEmail': '', 
                'authoredByUserName': '', 
                'authoredAt': None, 
                'reviewedByEmail': '', 
                'reviewedByUserName': '', 
                'reviewedAt': None, 
                'disabled': False, 
                'url': '98.233.edu', 
                'user_authored': None, 
                'user_reviewed': None
            }]
        }
        ```

### 创建 MergeRequest

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'mr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255|
|url|str|提交记录的链接|
|repo|int|提交记录所在仓库的 id|
|merge_id|int|合并请求的 id|
|description|str|合并请求的描述|
|state|str|合并请求的状态，"merged","closed","opened" 三选一|
|authoredByEmail|str|提请者的邮箱，最大长度 255，可选|
|authoredByUserName|str|提请者的名称，最大长度 255，可选|
|authoredAt|float|提请时间，可选|
|reviewedByEmail|str|复盘者的邮箱，最大长度 255，可选|
|reviewedByUserName|str|复盘者的名称，最大长度 255，可选|
|reviewedAt|float|复盘时间，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"mr",
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "a",
                    "url": "233.edu",
                    "repo": 2,
                    "merge_id":"2",
                    "description":"Some info",
                    "state":"merged"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 修改 MergeRequest

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'mr'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的合并请求的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255，可选|
|url|str|提交记录的链接，可选|
|repo|int|提交记录所在仓库的 id，可选|
|merge_id|int|合并请求的 id，可选|
|description|str|合并请求的描述，可选|
|state|str|合并请求的状态，"merged","closed","opened" 三选一，可选|
|authoredByEmail|str|提请者的邮箱，最大长度 255，可选|
|authoredByUserName|str|提请者的名称，最大长度 255，可选|
|authoredAt|float|提请时间，可选|
|reviewedByEmail|str|复盘者的邮箱，最大长度 255，可选|
|reviewedByUserName|str|复盘者的名称，最大长度 255，可选|
|reviewedAt|float|复盘时间，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"mr"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aa"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 MergeRequest

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'mr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的合并请求的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"mr"，
            "operation":"delete",
            "data":{
                 "id":1
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 Issue

### 查询 Issue

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'issue'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的议题列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"issue"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'issue_id': 1, 
                'repo': 1, 
                'title': 'First', 
                'description': 'first', 
                'state': 'closed', 
                'authoredByUserName': '', 
                'authoredAt': None, 
                'updatedAt': None, 
                'closedByUserName': '', 
                'closedAt': None, 
                'assigneeUserName': '', 
                'disabled': False, 
                'url': '233.edu', 
                'labels': '[]', 
                'is_bug': False, 
                'user_assignee': None, 
                'user_authored': None, 
                'user_closed': None
            }]
        }
        ```

### 创建 Issue

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255|
|url|str|提交记录的链接|
|repo|int|提交记录所在仓库的 id|
|issue_id|int|合并请求的 id|
|description|str|合并请求的描述|
|state|str|合并请求的状态，"closed","opened" 二选一|
|authoredByUserName|str|提请者的名称，最大长度 255，可选|
|authoredAt|float|提请时间，可选|
|updatedAt|float|更新时间，可选|
|closedByUserName|str|关闭者的名称，最大长度 255，可选|
|closedAt|float|关闭时间，可选|
|assigneeUserName|str|委托者的姓名，最大长度 255，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue",
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "a",
                    "url": "233.edu",
                    "repo": 2,
                    "issue_id":"2",
                    "description":"Some info",
                    "state":"opened"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 修改 Issue

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的议题的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|提交记录的标题，最大长度255，可选|
|url|str|提交记录的链接，可选|
|repo|int|提交记录所在仓库的 id，可选|
|issue_id|int|合并请求的 id，可选|
|description|str|合并请求的描述，可选|
|state|str|合并请求的状态，"closed","opened" 二选一，可选|
|authoredByUserName|str|提请者的名称，最大长度 255，可选|
|authoredAt|float|提请时间，可选|
|updatedAt|float|更新时间，可选|
|closedByUserName|str|关闭者的名称，最大长度 255，可选|
|closedAt|float|关闭时间，可选|
|assigneeUserName|str|委托者的姓名，最大长度 255，可选|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aa"
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 Issue

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的议题的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue"，
            "operation":"delete",
            "data":{
                 "id":1
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 CommitSRAssociation

### 查询 CommitSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'commit-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的 Commit-SR 关系列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"commit-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'commit': 1, 
                'SR': 1, 
                'auto_added': False
            }]
        }
        ```

### 创建 CommitSRAssociation

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'commit-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|commitId|int|关联的 commit 的 id|
|SRId|int|关联的 SR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue",
            "operation":"create",
            "data":{
                 "updateData": {
                     "commitId":1,
                     "SRId":2
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 CommitSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'commti-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|commitId|int|需要删除的提交记录的 id|
|SRId|int|需要删除的功能需求的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"commit-sr"，
            "operation":"delete",
            "data":{
                 "commitId":1,
                 "SRId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 MRSRAssociation

### 查询 MRSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'mr-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的 MR-SR 关系列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"mr-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'MR': 1, 
                'SR': 1, 
                'auto_added': False
            }]
        }
        ```

### 创建 MRSRAssociation

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'mr-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|MRId|int|关联的 MR 的 id|
|SRId|int|关联的 SR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"mr-sr",
            "operation":"create",
            "data":{
                 "updateData": {
                     "MRId":1,
                     "SRId":2
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 MRSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'mr-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|MRId|int|需要删除的合并请求的 id|
|SRId|int|需要删除的功能需求的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"mr-sr"，
            "operation":"delete",
            "data":{
                 "MRId":1,
                 "SRId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 IssueSRAssociation

### 查询 IssueSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'issue-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的 Issue-SR 关系列表|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"issue-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'issue': 1, 
                'SR': 1, 
                'auto_added': False
            }]
        }
        ```

### 创建 IssueSRAssociation

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|issueId|int|关联的 issue 的 id|
|SRId|int|关联的 SR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue-sr",
            "operation":"create",
            "data":{
                 "updateData": {
                     "issueId":1,
                     "SRId":2
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 IssueSRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|issueId|int|需要删除的议题的 id|
|SRId|int|需要删除的功能需求的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue-sr"，
            "operation":"delete",
            "data":{
                 "issueId":1,
                 "SRId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 IssueMRAssociation

### 查询 IssueMRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|repo|int|仓库 id|
|type|str|'issue-mr'|
|issueId|int|查询的议题的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|仓库内的对应 Issue 所有关的的 issue-MR 关系|

返回码说明：

|返回码|说明|
|-|-|
|0|查询成功|
|其他|查询失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "repo": 1,
            "type":"issue-mr",
            "issueId":1
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[{
                'id': 1, 
                'issue': 1, 
                'MR': 1, 
                'auto_added': False
            }]
        }
        ```

### 创建 IssueMRAssociation

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue-mr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|issueId|int|关联的 issue 的 id|
|MRId|int|关联的 MR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|创建成功|
|其他|创建失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue-mr",
            "operation":"create",
            "data":{
                 "updateData": {
                     "issueId":1,
                     "MRId":2
                }
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

### 删除 IssueMRAssociation

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'issue-mr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|issueId|int|需要删除的议题的 id|
|MRId|int|需要删除的功能需求的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|

返回码说明：

|返回码|说明|
|-|-|
|0|成功|
|其他|失败|

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type":"issue-mr"，
            "operation":"delete",
            "data":{
                 "issueId":1,
                 "MRId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```