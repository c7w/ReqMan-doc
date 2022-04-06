<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# RMS 模块模型接口
!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

!!! note "统一接口"
    有关 RMS 中模型的接口全部通过以下接口进行访问和操作，其中，查询操作通过 GET 请求，而创建、修改以及删除操作通过 POST 请求。

<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/project/</span>
</div>

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rms/project/</span>
</div>

## 操作 SR

### 查询 SR

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的功能需求列表|

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
            "type":"sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }
        ```

### 创建 SR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|功能需求的标题，最大长度255|
|description|str|功能需求的描述|
|priority|int|功能需求的优先级|
|rank|int|功能需求的序|
|state|str|功能需求的状态，"TODO" "WIP" "Reviewing" "Done" 四选一|

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
            "type":"sr"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "a",
                    "description": "sx",
                    "rank": 123,
                    "priority": 2,
                    "state": "TODO",
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

### 修改 SR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'sr'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的功能需求的 id|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|功能需求的标题，最大长度255，可选|
|description|str|功能需求的描述，可选|
|priority|int|功能需求的优先级，可选|
|rank|int|功能需求的序，可选|
|state|str|功能需求的状态，"TODO" "WIP" "Reviewing" "Done" 四选一，可选|

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
            "type":"sr"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aa",
                    "description": "bb",
                    "rank": 1325,
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

### 删除 SR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的功能需求的 id|

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
            "type":"sr"，
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

## 操作 IR

### 查询 IR

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'ir'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的原始需求列表|

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
            "type":"ir"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }
        ```

### 创建 IR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|原始需求的标题，最大长度255|
|description|str|原始需求的描述|
|rank|int|原始需求的序|

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
            "type":"ir"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "aa",
                    "description": "bb",
                    "rank": 132,

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

### 修改 IR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的原始需求的 id|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|原始需求的标题，最大长度255，可选|
|description|str|原始需求的描述，可选|
|rank|int|原始需求的序，可选|

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
            "type":"ir"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aaa",
                    "description": "bb",
                    "rank": 1325
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

### 删除 IR

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的原始需求的 id|

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
            "type":"ir"，
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