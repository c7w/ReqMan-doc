<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# RDTS 分页查询接口

对于 RDTS 中可能返回大量数据的查询，设置分页查询和单点查询接口。

!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

## 分页查询


### 分页 commit 查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_commits/</span>
</div>

分页查询项目中的 commit ，按创建时间倒序由新到旧排序。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_commits/?from=80&size=2&project=2&sessionId=RL0Waf4UdvMlXf48Gfvt415aMh4mUOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "payload": [
                    {
                        "id": 830,
                        "hash_id": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                        "repo": 2,
                        "title": "[SR.000.101.B] Fix wrongly selected eslint parser",
                        "commiter_email": "xxx@xxx.cn",
                        "commiter_name": "xxx",
                        "createdAt": 1647139676.0,
                        "user_committer": 1,
                        "additions": 65,
                        "deletions": 19
                    },
                    {
                        "id": 831,
                        "hash_id": "xxxxxxxxxxxxxxxxxxxxx",
                        "repo": 2,
                        "title": "[SR.000.101.F] Update frontend framework (#2)",
                        "commiter_email": "xxx@xxx.cn",
                        "commiter_name": "xxx",
                        "createdAt": 1647108898.0,
                        "user_committer": 1,
                        "additions": 146,
                        "deletions": 89
                    }
                ],
                "total_size": 1222,
                "from": 1220,
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
|payload|array\[objects\]|查询得到的记录|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|


### 分页 merge request 查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_merges/</span>
</div>

分页查询项目中的MR，按创建时间倒序由新到旧排序。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_merges/?from=80&size=2&project=2&sessionId=RL0Waf4UdvMlXf48Gfvt415aMh4mUOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "payload": [
                    {
                        "id": 348,
                        "merge_id": 94,
                        "repo": 3,
                        "title": "[SR.007.604] Update the right of QA (#87)",
                        "description": "",
                        "state": "merged",
                        "authoredByEmail": "",
                        "authoredByUserName": "xxx",
                        "authoredAt": 1651991536.508,
                        "reviewedByEmail": "",
                        "reviewedByUserName": "xxx",
                        "reviewedAt": 1651997052.699,
                        "disabled": false,
                        "url": "https://gitlab.secoder.net/xxx",
                        "user_authored": 26,
                        "user_reviewed": 26,
                        "updated_at": 1653149156.041643
                    },
                    {
                        "id": 347,
                        "merge_id": 93,
                        "repo": 3,
                        "title": "[SR.007.603] Bump Iteration Version (#86)",
                        "description": "#86",
                        "state": "merged",
                        "authoredByEmail": "",
                        "authoredByUserName": "xxx",
                        "authoredAt": 1651978752.412,
                        "reviewedByEmail": "",
                        "reviewedByUserName": "xxx",
                        "reviewedAt": 1651978824.625,
                        "disabled": false,
                        "url": "https://gitlab.secoder.net/xxx",
                        "user_authored": 1,
                        "user_reviewed": 1,
                        "updated_at": 1653149156.041643
                    }
                ],
                "total_size": 316,
                "from": 80,
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
|payload|array\[objects\]|查询得到的记录|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|

### 分页 issue 查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_bugs/</span>
</div>

分页查询项目中的 issue ，按创建时间倒序由新到旧排序。注意，只有打上了 `bug` 标签的 issue 才会被当做缺陷。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_bugs/?from=80&size=2&project=2&sessionId=RL0Waf4UdvMlXf48Gfvt415aMh4mUOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "payload": [
                    {
                        "id": 999,
                        "issue_id": 222,
                        "repo": 2,
                        "title": "[SR.006.108] 项目 IR List 关联 SR filter 时出错",
                        "description": "[SR.006.108] 项目 IR List 关联 SR filter 时出错",
                        "state": "closed",
                        "authoredByUserName": "xxxxxxxxx",
                        "authoredAt": 1650946534.982,
                        "updatedAt": 1653149725.742544,
                        "closedByUserName": "xxxxxxxxxx",
                        "closedAt": 1651034863.203,
                        "assigneeUserName": "xxxxxxxxxx",
                        "disabled": false,
                        "url": "https://gitlab.secoder.net/xxxxxxxxxx",
                        "labels": "[\"bug\"]",
                        "is_bug": true,
                        "user_assignee": 15,
                        "user_authored": 1,
                        "user_closed": 20
                    }
                ],
                "total_size": 188,
                "from": 20,
                "data_size": 1,
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
|payload|array\[objects\]|查询得到的记录|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|


## 单点查询

在请求页面数据时，可能需要请求部分相关数据，所以提供了单点查询接口。

### 单个 commit 查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_single_commit/</span>
</div>

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_single_commit/?id=2243&project=2&sessionId=rx9LLDqTdepC3oTXyv16LsEv5gNhSgKs
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "id": 2243,
                "hash_id": "xxxxxxxxxxxxxxxxxxxxxx",
                "repo": 2,
                "title": "[SR.007.129] Remove RDTS Timer, add refresh by hand (#130)",
                "message": "[SR.007.129] Remove RDTS Timer, add refresh by hand (#130)\n",
                "commiter_email": "xxxxxxxx@example.com",
                "commiter_name": "xxxxxxxxxx",
                "createdAt": 1652240435.0,
                "url": "https://gitlab.secoder.net/xxxxxxxxxxxx",
                "disabled": false,
                "commite_date": 1652240477,
                "user_committer": 1,
                "additions": 38,
                "deletions": 33
            }
        }
        ```

### 单个 merge request 查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_single_merge/</span>
</div>

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_single_merge/?id=553&project=2&sessionId=rx9LLDqTdepC3oTXyv16LsEv5gNhSgKs
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "id": 553,
                "merge_id": 133,
                "repo": 2,
                "title": "[SR.007.129] Remove RDTS Timer, add refresh by hand (#130)",
                "description": "",
                "state": "merged",
                "authoredByEmail": "",
                "authoredByUserName": "xxxxxxxxxxxx",
                "authoredAt": 1652240452.864,
                "reviewedByEmail": "",
                "reviewedByUserName": "xxxxxxxxxx",
                "reviewedAt": 1652240542.406,
                "disabled": false,
                "url": "https://gitlab.secoder.net/xxxxxxxxxxxx",
                "user_authored": 1,
                "user_reviewed": 1,
                "updated_at": 1653194972.046079
            }
        }
        ```

### 单个 issue 查询

这里仅查询带 bug 标签的 issue。

<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/project_single_bug/</span>
</div>

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/project_single_bug/?id=527&project=2&sessionId=rx9LLDqTdepC3oTXyv16LsEv5gNhSgKs
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "id": 527,
                "issue_id": 130,
                "repo": 2,
                "title": "[SR.007.129] 修表格刷新",
                "description": "",
                "state": "closed",
                "authoredByUserName": "xxxxxxxxxxxx",
                "authoredAt": 1652240115.629,
                "updatedAt": 1653194966.832546,
                "closedByUserName": "xxxxxxxxxxxx",
                "closedAt": 1652240566.753,
                "assigneeUserName": "xxxxxxxxxxxx",
                "disabled": false,
                "url": "https://gitlab.secoder.net/xxxxxxxxxxxx",
                "labels": "[\"bug\"]",
                "is_bug": true,
                "user_assignee": 1,
                "user_authored": 1,
                "user_closed": 1
            }
        }
        ```


单点查询的请求和响应模式较为类似，下面进行统一说明。

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|id|int/str|需要查询的 commit / merge request / issue 的 ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为不存在这条记录|

#### 响应数据
响应数据是一个 `object` ，即被查询的对象。