<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# RDTS 分页查询接口

对于 RDTS 中可能返回大量数据的查询，设置分页查询接口。

!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

## 分页查询

### 分页Merge Request查询
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/project_merges/</span>
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
|payload|array\[objects\]|查询得到的SR|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|

### 分页 commit 查询
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/project_commits/</span>
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
|payload|array\[objects\]|查询得到的SR|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|

### 分页 issue 查询
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/project_bugs/</span>
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
|payload|array\[objects\]|查询得到的SR|
|total_size|int|全部记录的条数|
|data_size|int|本页记录的条数|
|from|int|开始位置|
|related_set|object|本次查询中相关集合，本接口恒为 `null`|
