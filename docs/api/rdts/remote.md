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

用于创建或修改远程仓库。

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


### 同步记录查询
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/repo_crawllog/</span>
</div>

用于查询远程仓库的同步记录。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/repo_crawllog/?sessionId=Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74&repo=1&project=1&page=1
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": [
                {
                    "id": 124,
                    "time": 1649840324.738104,
                    "status": 200,
                    "message": "",
                    "request_type": "merge",
                    "finished": true,
                    "updated": false
                },
                {
                    "id": 123,
                    "time": 1649840304.765841,
                    "status": 200,
                    "message": "",
                    "request_type": "commit",
                    "finished": false,
                    "updated": false
                },
                ...
            ]
        }
        ```

#### 请求权限 
项目管理员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|repo|int/str|仓库ID|
|page|int/str|记录页码|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为仓库不存在|

#### 响应数据
响应数据是一个数组，数组各字段如下

|字段|类型|说明|
|-|-|-|
|id|int|同步ID|
|time|float|同步开始时间，以秒计|
|status|int|同步状态，以远程服务器的HTTP状态码表示|
|message|int|错误信息，如果同步状态不为200，则记录远程服务器返回的错误信息|
|request_type|str|同步内容，为 "merge" , "issue" 或 "commit"|
|finished|bool|表示本次同步的内容是否已经处理完|
|updated|bool|表示本次同步是否进行了更新，包括对已有记录的增加、修改、删除，不包括已有记录和SR, MR的匹配|

!!! info "同步过程"
    同步时会先向远程服务器逐页请求数据，然后处理服务器返回的数据

    - 如果逐页请求的过程中出现了一次失败，则记录同步状态和出错信息，本次同步终止
    - 如果逐页请求成功，则标记同步状态为200，finished=false，开始处理数据
    - 所有数据处理结束后，finished=true，本次同步结束


### 同步记录详情
<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/crawl_detail/</span>
</div>

用于查询某次同步的修改情况。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/crawl_detail/?sessionId=Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74&crawl_id=106&project=1&page=1
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "log": {
                    "id": 91,
                    "repo": 1,
                    "time": 1649829297.896791,
                    "status": 200,
                    "message": "",
                    "request_type": "commit",
                    "finished": true,
                    "updated": true
                },
                "issue": [],
                "merge": [],
                "commit": [
                    {
                        "hash_id": "4501fa5929d130f36b997276728b9dc47f2899bb",
                        "title": "[SR.003.906] Bugfix: scheduler & dev database  (#30)",
                        "commiter_name": "c7w",
                        "op": "update"
                    }
                ]
            }
        }
        ```

#### 请求权限 
项目管理员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|crwal_id|int/str|同步ID|
|page|int/str|页码|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为项目中没有这条同步记录|

#### 响应数据
相应数据是一个数组，数组各字段介绍如下
|字段|类型|说明|
|-|-|-|
|log|object|同步基本信息|
|commit/merge/issue|array|同步的内容及操作，操作包括 "add"，"remove"，"update"|

!!! info "注意"
    commit/merge/issue三个数组至多有1个是非空的，因为单次同步仅请求一种数据。

## 数据分析

### 开发过程数据
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/get_recent_acitvity/</span>
</div>

用于质保工程师查看指定开发工程师的近期或全部MergeRequest，代码行数，Issue等数据情况。

??? example "示例"
    === "请求"
        查询 `id` 为 `1` 的开发工程师在近1周内的开发情况，展示详情。
        ```json
        {
            "sessionId": "Rd8Gs0jw0jdbUeJzf7EIBwkwr7aYit74",
            "project": 1,
            "digest": false,
            "dev_id": 1,
            "limit": 604800
        }
        ```
    === "响应 1"
        请求中 `digest=true` 时
        ```json
        {
            "code": 0,
            "data": {
                // TODO: add a refreshed example
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
- <span>**[ GET ]** &nbsp;&nbsp; /rdts/iteration_bugs/</span>
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
