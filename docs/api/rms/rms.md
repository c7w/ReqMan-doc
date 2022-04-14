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

### 修改 SR 整体

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
|updateData|object|所修改的内容|

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

### 修改 SR 状态

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'SRState'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的功能需求的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
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
                    "state":"TODO"
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
|updateData|object|所修改的内容|

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

## 操作 Iteration

### 查询 Iteration

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'iteration'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的迭代列表|

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
            "type":"iteration"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }
        ```

### 创建 Iteration

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'iteration'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|迭代的标题，最大长度255|
|sid|int|迭代的 sid|
|begin|float|迭代的开始时间|
|end|float|迭代的结束时间|

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
            "type":"iteration"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "w",
                    "sid": 21,
                    "begin": 123.0,
                    "end": 2.0
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

### 修改 Iteration

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'iteration'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的迭代的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|迭代的标题，最大长度255，可选|
|sid|int|迭代的 sid，可选|
|begin|float|迭代的开始时间，可选|
|end|float|迭代的结束时间，可选|

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
            "type":"iteration"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aaa",
                    "sid": 12,
                    "state": "Done",
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

### 删除 Iteration

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'iteration'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的迭代的 id|

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
            "type":"iteration"，
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

## 操作 Service

### 查询 Service

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'service'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的服务列表|

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
            "type":"service"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }
        ```

### 创建 Service

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'service'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|服务的标题，最大长度255|
|sid|int|服务的 sid|
|rank|int|服务的序|

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
            "type":"service"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "title": "12a",
                    "description": "d",
                    "rank": 12
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

### 修改 Service

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'service'|
|operation|str|'update'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要修改的服务的 id|
|updateData|object|所修改的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|title|str|服务的标题，最大长度255，可选|
|sid|int|服务的 sid，可选|
|rank|int|服务的序，可选|

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
            "type":"service"，
            "operation":"update",
            "data":{
                 "id":1,
                 "updateData": {
                    "title": "aass",
                    "description": "abb",
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

### 删除 Service

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'service'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|id|int|需要删除的服务的 id|

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
            "type":"service"，
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

## 操作 SR_Change_Log

### 查询 SR_Change_Log

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'SR_changeLog'|
|SRId|int|查询的 SR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|所查询的SR的变更记录列表|

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
            "type": "SR_changeLog",
            "SRId":2,
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }
        ```

## 操作 user-iteration 联合关系

### 查询 user-iteration 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'user-iteration'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的用户-迭代关系列表|

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
            "type":"user-iteration"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 user-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'user-iteration'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|userId|int|建立关系的用户的 id|
|iterationId|int|建立关系的迭代的 id|

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
            "type":"user-iteration"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "userId":1,
                    "iterationId":2
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

### 删除 user-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'user-iteration'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|userId|int|需要删除的关系所关联的用户的 id|
|iterationsId|int|需要删除的关系所关联的迭代的 id|

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
            "type":"user-iteration"，
            "operation":"delete",
            "data":{
                 "userId":1,
                 "iterationId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 ir-sr 联合关系

### 查询 ir-sr 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'ir-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的原始需求-功能需求关系列表|

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
            "type":"ir-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 ir-sr 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|IRId|int|建立关系的原始需求的 id|
|SRId|int|建立关系的功能需求的 id|

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
            "type":"ir-sr"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "SRId":1,
                    "IRId":2
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

### 删除 ir-sr 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|IRId|int|需要删除的关系所关联的原始需求的 id|
|SRId|int|需要删除的关系所关联的功能需求的 id|

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
            "type":"ir-sr"，
            "operation":"delete",
            "data":{
                 "IRId":1,
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

## 操作 sr-iteration 联合关系

### 查询 sr-iteration 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'sr-iteration'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的功能需求-迭代关系列表|

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
            "type":"sr-iteration"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 sr-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'sr-iteration'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|建立关系的迭代的 id|
|SRId|int|建立关系的功能需求的 id|

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
            "type":"sr-iteration"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "SRId":1,
                    "iterationId":2
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

### 删除 sr-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'sr-iteration'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|需要删除的关系所关联的迭代的 id|
|SRId|int|需要删除的关系所关联的功能需求的 id|

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
            "type":"sr-iteration"，
            "operation":"delete",
            "data":{
                 "iterationId":1,
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

## 操作 service-sr 联合关系

### 查询所有 service-sr 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'service-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的全部服务-功能需求关系列表|

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
            "type":"service-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 查询 SR 对应的 service

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'serviceOfSR'|
|SRId|int|查询的 SR 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|服务-功能需求关系列表，有且只有一条|

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
            "type":"serviceOfSR",
            "SRId":2
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 查询  service 对应的 SR

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'SROfService'|
|serviceId|int|查询的 service 的 id|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|服务-功能需求关系列表|

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
            "type":"SROfService",
            "serviceId":2
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 sr-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'service-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|serviceId|int|建立关系的服务的 id|
|SRId|int|建立关系的功能需求的 id|

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
            "type":"service-sr"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "SRId":1,
                    "serviceId":2
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

### 删除 service-sr 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'service-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|serviceId|int|需要删除的关系所关联的服务的 id|
|SRId|int|需要删除的关系所关联的功能需求的 id|

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
            "type":"service-sr"，
            "operation":"delete",
            "data":{
                 "serviceId":1,
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

## 操作 ir-iteration 联合关系

### 查询 ir-iteration 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'ir-iteration'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的原始需求-迭代关系列表|

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
            "type":"ir-iteration"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 ir-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir-iteration'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|建立关系的迭代的 id|
|IRId|int|建立关系的原始需求的 id|

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
            "type":"ir-iteration"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "IRId":1,
                    "iterationId":2
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

### 删除 ir-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'ir-iteration'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|需要删除的关系所关联的迭代的 id|
|IRId|int|需要删除的关系所关联的原始需求的 id|

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
            "type":"ir-iteration"，
            "operation":"delete",
            "data":{
                 "iterationId":1,
                 "IRId":2
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 project-iteration 联合关系

### 查询 project-iteration 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'project-iteration'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的项目-迭代关系列表|

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
            "type":"project-iteration"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 project-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'project-iteration'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|建立关系的迭代的 id|

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
            "type":"project-iteration"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "iterationId":2
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

### 删除 project-iteration 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'project-iteration'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|iterationId|int|需要删除的关系所关联的迭代的 id|

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
            "type":"project-iteration"，
            "operation":"delete",
            "data":{
                 "iterationId":1
            }
        }
        ```
    === "响应"
        ```json
        {
            "code": 0
        }
        ```

## 操作 user-sr 联合关系

### 查询 user-sr 联合关系

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int|项目 id|
|type|str|'user-sr'|

#### 响应状态
|参数|类型|说明|
|-|-|-|
|code|int|返回码|
|data|list|项目内的用户-功能需求列表|

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
            "type":"user-sr"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data":[]
        }

### 创建 user-sr 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'user-sr'|
|operation|str|'create'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|updateData|object|所创建的内容|

updateData 内容说明：

|项目|类型|说明|
|-|-|-|
|userId|int|建立关系的用户的 id|
|SRId|int|建立关系的功能需求的 id|

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
            "type":"user-sr"，
            "operation":"create",
            "data":{
                 "updateData": {
                    "SRId":1,
                    "userId":2
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

### 删除 user-sr 联合关系

#### 请求参数

|参数|类型|说明|
|-|-|-|
|project|int|项目id|
|type|str|'user-sr'|
|operation|str|'delete'|
|data|object|操作的详细信息|

data 内容说明：

|项目|类型|说明|
|-|-|-|
|userId|int|需要删除的关系所关联的用户的 id|
|SRId|int|需要删除的关系所关联的功能需求的 id|

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
            "type":"user-sr"，
            "operation":"delete",
            "data":{
                 "userId":1,
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