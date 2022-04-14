<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>
# 远程仓库和开发数据

## 远程仓库

### 创建和修改远程仓库
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/repo_op/</span>
</div>

用于质保工程师查看指定开发工程师的近期或全部MergeRequest，代码行数，Issue等数据情况。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "type": "gitlab",
            "remote_id": "791",
            "access_token": "vzDY55aaSA-5rjeeYYxxn",
            "enable_crawling": true,
            "info": {
                "base_url": "https://gitlab.secoder.net"
            },
            "title": "Hello! Repo Sync!",
            "description": "A remote gitlab repo",
            "op": "add"
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
|type|str|远程仓库类型，这里只实现了gitlab|
|remote_id|str|远程仓库ID|
|access_token|str|远程仓库访问令牌|
|enable_crawling|bool|是否同步该远程仓库|
|info|object|该远程仓库需要的其他信息，必须实现 `base_url`属性，表示接口的地址|
|title|str|仓库的名称|
|description|str|仓库的描述|
|op|str|操作，只能为 `add` 或 `modify`|

!!! info "如何同步其他类型的远程仓库"
    - 在 `/rdts/query_class.py` 中继承 `RemoteRepoFetcher` 并实现其接口
    - 将新类型添加到 `rdts.query_class.type_map` 中
    这样就定义了新的远程仓库，其类型是 `type_map` 中定义的键名。

#### 响应状态
|状态码|说明|
|-|-|
|0|操作成功|
|1|操作失败，因为项目名称长于255字符|
|2|操作失败，因为项目描述长于1000字符|
|3|修改失败，因为这个仓库ID不存在|



## 数据分析

### 开发过程数据
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/get_recent_acitvity/</span>
</div>

用于质保工程师查看指定开发工程师的近期或全部MergeRequest，代码行数，Issue等数据情况。

??? example "示例"
    === "请求"
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "digest": false,
            "dev_id": 1,
            "limit": 604800
        }
        ```
        查询 `id` 为 `1` 的开发工程师在近1周内的开发情况，展示详情。
    === "响应 1"
        请求中 `digest=true` 时
        ```json
        {
            "code": 0,
            "data": {
                "mr_count": 3,
                "commit_count": 12,
                "additions": 1984,
                "deletions": 647,
                "issue_count": 5,
                "issue_times": [39794, 191275, 63799, 151260, 83450]
            }
        }
        ```
    === "响应 2"
        请求中 `digest=false` 时
        ```json
        {
            "code": 0,
            "data": {
                "merges": [
                    {
                        "id": 4,
                        "merge_id": 34,
                        "title": "[SR.003.604.F] Merge: modify password, upload avatar (#34)",
                        "url": "https://gitlab.secoder.net/undefined/backend/-/merge_requests/34",
                        "repo": "backend"
                    }
                ],
                "commits": [
                    {
                        "id": 203,
                        "hash_id": "8fbe227a8a37a4fe2c41f07a1e2d09c1b3086667",
                        "message": "[SR.004.605.I] Disabled unfinished modules (#52)\n",
                        "createdAt": 1649511580.0,
                        "url": "https://gitlab.secoder.net/undefined/backend/-/commit/8fbe227a8a37a4fe2c41f07a1e2d09c1b3086667",
                        "additions": 15
                        "deletions": 46
                        "repo": "backend"
                    }
                ],
                "issues": [
                    {
                        "id": 1,
                        "issue_id": 39,
                        "title": "[SR.003.609] 检修 Schedule Task",
                        "authoredAt": 1648569415.762,
                        "closedAt": 1648609209.383,
                        "repo": "backend"
                    }
                ],

            }
        }
        ```

#### 请求权限 
项目管理员，质保工程师

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|digest|bool|如果为 `true`，则只显示数值摘要，否则MergeRequest, Issue 和 commit的详细信息|
|dev_id|int/str|被查询的开发工程师ID|
|limit|int/str|查询时间段，用从查询时刻起，向前回溯的秒数表示|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为项目中不存在这个工程师|

#### 响应数据
##### 当请求 `digest=true` 时
|字段|类型|说明|
|-|-|-|
|*_count|int|被查询对象的数量，其中issue仅包含已经关闭的issue|
|addition|int|新增行数之和|
|deletion|int|删除行数之和|
|issue_times|array\[int\]|每个issue的解决时间，以秒计算|

##### 当请求 `digest=false` 时
各字段含义十分明确，详见示例。



### 迭代周期bug情况
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /ums/iteration_bugs/</span>
</div>

用于质保工程师查看指定迭代周期的bug情况。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/iteration_bugs/?sessionId=Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74&project=1&iteration=1
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "bug_issues": [
                    {
                        "id": 14,
                        "issue_id": 26,
                        "title": "[SR.003.903] Inspect the proxy model of secoder",
                        "description": "the terminal do not even have a vim ?!?!?",
                        "authoredAt": 1648118598.553,
                        "closedAt": 1648196852.61,
                        "url": "https://gitlab.secoder.net/undefined/backend/-/issues/26",
                        "sr": {
                            "id": 1,
                            "title": "[SR.003.903] 调整后端配置，修复流量控制中的代理问题",
                            "description": "检查后端代理配置",
                            "priority": 1,
                            "rank": 1
                        }
                    }
                ]
            }
        }
        ```

#### 请求权限 
项目管理员，质保工程师

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|iteration|int/str|迭代周期ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为项目中不存在这个迭代周期|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|bug_issues|array\[int\]|该迭代周期中所有被标记为bug的issue的详情，及其关联SR的相关信息|
