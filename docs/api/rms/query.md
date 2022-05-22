<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# RMS 关联查询接口

!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

## SR 基础查询

### 分页SR查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/project_sr/</span>
</div>

分页查询项目中的SR，按创建时间倒序由新到旧排序。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rms/project_sr/?from=0&size=2&project=2&sessionId=6gr6q4MAo9l7fk3fl6s5SrK1Ukjx9g1A
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "payload": [
                    {
                        "id": 22371,
                        "project": 2,
                        "title": "e",
                        "description": "e",
                        "priority": 1,
                        "rank": 1,
                        "state": "Reviewing",
                        "createdBy": 17,
                        "createdAt": 1652368032.168103,
                        "IR": null
                    },
                    {
                        "id": 22364,
                        "project": 2,
                        "title": "SR.007.126",
                        "description": "修复日程表1111",
                        "priority": 1,
                        "rank": 1,
                        "state": "Done",
                        "createdBy": 1,
                        "createdAt": 1652196495.978615,
                        "IR": null
                    }
                ],
                "total_size": 90,
                "from": 0,
                "data_size": 2,
                "related_set": null
            }
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|from|int/str|查询起始位置|
|size|int/str|页面大小，超过100将被截断到100|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|payload|array\[objects\]|查询得到的SR|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|


### 单条SR查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/project_sr/</span>
</div>

按ID查询某条SR的详细信息。

??? example "示例"
    === "请求"
        ```bash
        curl https://backend-undefined.app.secoder.net/rms/project_single_sr/?id=48279&project=2&sessionId=RL0Waf4UdvMlXf48Gfvt415aM775UOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "id": 48279,
                "project": 2,
                "title": "SR.007.907",
                "description": "后端其他错误修复",
                "priority": 1,
                "rank": 1,
                "state": "Done",
                "createdBy": 15,
                "createdAt": 1652460440.731574,
                "IR": [],
                "commit": [
                    [
                        {
                            "id": 115844,
                            "hash_id": "d6f350ff896f3898cef4ab384f2dcfb80e60bf96",
                            "repo": 3,
                            "title": "[SR.007.907] Merge: backend bugfix push (#102)",
                            "message": "[SR.007.907] Merge: backend bugfix push (#102)",
                            "commiter_email": "2020011156@secoder.net",
                            "commiter_name": "2020011156@secoder.net",
                            "createdAt": 1652502430.0,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/commit/d6f350ff896f3898cef4ab384f2dcfb80e60bf96",
                            "commite_date": 0.0,
                            "user_committer": 29,
                            "additions": 59,
                            "deletions": 10
                        },
                        true
                    ],
                    [
                        {
                            "id": 115972,
                            "hash_id": "d90993abdd0c0e03341357460e059d8251e7786e",
                            "repo": 3,
                            "title": "[SR.007.907] Push a new bugfix in crawler (#102)",
                            "message": "[SR.007.907] Push a new bugfix in crawler (#102)",
                            "commiter_email": "2020011156@secoder.net",
                            "commiter_name": "2020011156@secoder.net",
                            "createdAt": 1652524585.0,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/commit/d90993abdd0c0e03341357460e059d8251e7786e",
                            "commite_date": 0.0,
                            "user_committer": 29,
                            "additions": 1,
                            "deletions": 1
                        },
                        true
                    ],
                    [
                        {
                            "id": 115969,
                            "hash_id": "0c31e50e9902daeb97e858e365cf76372398f761",
                            "repo": 3,
                            "title": "[SR.007.907.B] Merge: fix a page num incorrection (#102)",
                            "message": "[SR.007.907.B] Merge: fix a page num incorrection (#102)",
                            "commiter_email": "2020011156@secoder.net",
                            "commiter_name": "刘明道",
                            "createdAt": 1652524383.0,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/commit/0c31e50e9902daeb97e858e365cf76372398f761",
                            "commite_date": 0.0,
                            "user_committer": 29,
                            "additions": 1,
                            "deletions": 1
                        },
                        true
                    ],
                    [
                        {
                            "id": 115968,
                            "hash_id": "7d135ffae012f8f2c154c515b07cf6e5532f0b41",
                            "repo": 3,
                            "title": "[SR.007.907.B] Fix a page num incorrection (#102)",
                            "message": "[SR.007.907.B] Fix a page num incorrection (#102)\n",
                            "commiter_email": "2020011156@secoder.net",
                            "commiter_name": "刘明道",
                            "createdAt": 1652524161.0,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/commit/7d135ffae012f8f2c154c515b07cf6e5532f0b41",
                            "commite_date": 0.0,
                            "user_committer": 29,
                            "additions": 1,
                            "deletions": 1
                        },
                        true
                    ]
                ],
                "issue": [
                    [
                        {
                            "id": 556,
                            "issue_id": 102,
                            "repo": 3,
                            "title": "[SR.007.907] 后端其他错误修复",
                            "description": "- create 后没有刷出高优先队列的问题\n- webhook和crawl同时请求的并发错误",
                            "state": "closed",
                            "authoredByUserName": "2020011156",
                            "authoredAt": 1652436125.837,
                            "updatedAt": 1653143391.619671,
                            "closedByUserName": "2020011156",
                            "closedAt": 1652505135.897,
                            "assigneeUserName": "2020011156",
                            "disabled": false,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/issues/102",
                            "labels": "[\"bug\"]",
                            "is_bug": true,
                            "user_assignee": 29,
                            "user_authored": 29,
                            "user_closed": 29
                        },
                        true
                    ]
                ],
                "mr": [
                    [
                        {
                            "id": 789,
                            "merge_id": 122,
                            "repo": 3,
                            "title": "[SR.007.907] Push a new bugfix in crawler (#102)",
                            "description": "",
                            "state": "merged",
                            "authoredByEmail": "",
                            "authoredByUserName": "2020011156",
                            "authoredAt": 1652524384.633,
                            "reviewedByEmail": "",
                            "reviewedByUserName": "2020011156",
                            "reviewedAt": 1652524586.065,
                            "disabled": false,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/merge_requests/122",
                            "user_authored": 29,
                            "user_reviewed": 29,
                            "updated_at": 1653143395.875232
                        },
                        true
                    ],
                    [
                        {
                            "id": 787,
                            "merge_id": 121,
                            "repo": 3,
                            "title": "[SR.007.907.B] Merge: fix a page num incorrection (#102)",
                            "description": "",
                            "state": "merged",
                            "authoredByEmail": "",
                            "authoredByUserName": "2020011156",
                            "authoredAt": 1652524180.455,
                            "reviewedByEmail": "",
                            "reviewedByUserName": "2020011156",
                            "reviewedAt": 1652524384.045,
                            "disabled": false,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/merge_requests/121",
                            "user_authored": 29,
                            "user_reviewed": 29,
                            "updated_at": 1653143395.875232
                        },
                        true
                    ],
                    [
                        {
                            "id": 620,
                            "merge_id": 120,
                            "repo": 3,
                            "title": "[SR.007.907.I] Fix updated error when frequently change objects (#102)",
                            "description": "",
                            "state": "merged",
                            "authoredByEmail": "",
                            "authoredByUserName": "2020011156",
                            "authoredAt": 1652488399.253,
                            "reviewedByEmail": "",
                            "reviewedByUserName": "2020011156",
                            "reviewedAt": 1652493227.995,
                            "disabled": false,
                            "url": "https://gitlab.secoder.net/undefined/backend/-/merge_requests/120",
                            "user_authored": 29,
                            "user_reviewed": 29,
                            "updated_at": 1653143395.875232
                        },
                        true
                    ]
                ]
            }
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|id|int/str|待查询SR的ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|SR不存在|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|commit/issue/mr|array\[objects\]|与SR关联的commit/issue(bug)/mr|
其余内容是SR基本信息，含义详见模型。



## 关联集合查询

### 查询与IR相关的SR集合
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/get_ir_sr/</span>
</div>

找到与IR相关的SR。

### 查询与服务相关的SR集合
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/get_service_sr/</span>
</div>

找到与服务相关的SR。

### 查询与迭代相关的SR集合
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/get_iteration_sr/</span>
</div>

找到与迭代相关的SR。


以查询与服务相关的SR集合为例。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rms/get_service_sr/?project=2&service=4&sessionId=RL0Waf4UdvMlXf48Gfvtd25w5h4mUOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": [
                {
                    "id": 39,
                    "title": "SR.003.104",
                    "description": "项目列表部分逻辑",
                    "priority": 1,
                    "state": "TODO",
                    "createdBy": 15,
                    "createdAt": 1650035404.850554
                },
                {
                    "id": 49,
                    "title": "SR.003.113",
                    "description": "列表前端逻辑层",
                    "priority": 1,
                    "state": "Done",
                    "createdBy": 15,
                    "createdAt": 1650035527.753519
                }
            ]
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|service/ir/iteration|int/str|查询的服务/IR/迭代的ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|

#### 响应数据

响应数据是一个数组；如果找不得结果，则返回空数组。


### 查询特定用户的SR集合

<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/dashboard/</span>
</div>

查询用户在项目中负责的未开始、开发中和测试中的SR，用于前端展示 Dashboard 。被查询的用户由登录的 sessionId 确定。

对于每个类型的SR，优先级高的前；相同优先级的，后创建的SR在前。前端课可以

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rms/dashboard/?limit=10&project=2&sessionId=6gr6q4MAo9l6jJxfl6s5S52nc72x9g1A
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "wip": [
                    {
                        "id": 22383,
                        "project": 2,
                        "title": "SR.007.123",
                        "description": "helllo",
                        "priority": 1,
                        "rank": 1,
                        "state": "WIP",
                        "createdBy": 1,
                        "createdAt": 1652410722.582857,
                        "disabled": false
                    }
                ],
                "todo": [
                    {
                        "id": 35699,
                        "project": 2,
                        "title": "eee",
                        "description": "",
                        "priority": 1,
                        "rank": 1,
                        "state": "TODO",
                        "createdBy": 17,
                        "createdAt": 1652501639.480391,
                        "disabled": false
                    }
                ],
                "reviewing": []
            }
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|limit|int/str|每种类型SR的返回数量|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|todo|array\[objects\]|未开始的SR|
|wip|array\[objects\]|开发中的SR|
|reviewing|array\[objects\]|测试中的SR|

### 查询特定关键词的SR集合
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rms/search_sr/</span>
</div>

按关键词查询SR，便于添加IR-SR关联时进行搜素。

??? example "示例"
    === "请求"
        ```json
        {
            "project": 2,
            "title_only": true,
            "kw": "007",
            "limit": 5,
            "sessionId": "6gr6q4MAo9l6jJxfl6s5SrK1Ukjx9g1A"
        }
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": [
                {
                    "id": 22383,
                    "title": "SR.007.123",
                    "description": "",
                    "state": "Done",
                    "createdBy": 1,
                    "createdAt": 1652410722.582857
                },
                {
                    "id": 22364,
                    "title": "SR.007.126",
                    "description": "修复日程表1111",
                    "state": "Done",
                    "createdBy": 1,
                    "createdAt": 1652196495.978615
                }
            ]
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|title_only|bool/str|是否仅在标题中检索|
|kw|str|关键词|
|limit|int/str|截取条数|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|

#### 响应数据

响应数据是一个数组，其中包含按关键词和匹配方式找到的所有结果。如果找不得结果，则返回空数组。
