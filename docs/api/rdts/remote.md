<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>
# 远程仓库和开发数据



!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

    [权限约定](../ums/index.md#_1)中所提及的状态码在本节不再重复。

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
            "access_token": "vzDY55aaSA3-5rjeeYYxxn",
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
|type|str|远程仓库类型，这里只实现了`gitlab`|
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


### Webhook 推送接口

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/webhook/</span>
</div>


用于接受远程仓库更新的推送。

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


### 测试远程仓库是否正确配置
<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rdts/test_access_token/</span>
</div>

用于在用户新建项目后，查看项目是否能正确和远程仓库对接。

该接口会使用用户提供的ID和Access-Token向远程仓库请求项目信息，将远程仓库的响应进行转发，便于用户判断自己是否正确配置了远程仓库。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/test_access_token/?project=26&repository=16&sessionId=b4RCDE7ovT5IMQrV2JnyCZicJN8j8kD8
        ```
    === "响应1"
        ```json
        {
            "code":0,
            "data":{
                "status":401,
                "response":
                {
                    "message":"401 Unauthorized"
                }
            }
        }
        ```
    === "响应2"
        ```json
        {
            "code": 0,
            "data": {
                "status": 200,
                "response": {
                    "id": 469,
                    "description": null,
                    "name": "backend",
                    "name_with_namespace": "undefined / backend",
                    "path": "backend",
                    "path_with_namespace": "undefined/backend",
                    "created_at": "2022-03-11T09:11:58.942Z",
                    "default_branch": "dev",
                    "tag_list": [],
                    "ssh_url_to_repo": "git@gitlab.secoder.net:undefined/backend.git",
                    "http_url_to_repo": "https://gitlab.secoder.net/undefined/backend.git",
                    "web_url": "https://gitlab.secoder.net/undefined/backend",
                    "readme_url": "https://gitlab.secoder.net/undefined/backend/-/blob/dev/readme.md",
                    "avatar_url": null,
                    "forks_count": 0,
                    "star_count": 0,
                    "last_activity_at": "2022-04-26T02:05:35.147Z",
                    "namespace": {
                        "id": 271,
                        "name": "undefined",
                        "path": "undefined",
                        "kind": "group",
                        "full_path": "undefined",
                        "parent_id": null,
                        "avatar_url": null,
                        "web_url": "https://gitlab.secoder.net/groups/undefined"
                    },
                    "_links": {
                        "self": "https://gitlab.secoder.net/api/v4/projects/469",
                        "issues": "https://gitlab.secoder.net/api/v4/projects/469/issues",
                        "merge_requests": "https://gitlab.secoder.net/api/v4/projects/469/merge_requests",
                        "repo_branches": "https://gitlab.secoder.net/api/v4/projects/469/repository/branches",
                        "labels": "https://gitlab.secoder.net/api/v4/projects/469/labels",
                        "events": "https://gitlab.secoder.net/api/v4/projects/469/events",
                        "members": "https://gitlab.secoder.net/api/v4/projects/469/members"
                    },
                    "packages_enabled": true,
                    "empty_repo": false,
                    "archived": false,
                    "visibility": "private",
                    "resolve_outdated_diff_discussions": false,
                    "container_registry_enabled": true,
                    "container_expiration_policy": {
                        "cadence": "1d",
                        "enabled": true,
                        "keep_n": 10,
                        "older_than": "90d",
                        "name_regex": null,
                        "name_regex_keep": null,
                        "next_run_at": "2022-04-26T12:50:03.386Z"
                    },
                    "issues_enabled": true,
                    "merge_requests_enabled": true,
                    "wiki_enabled": true,
                    "jobs_enabled": true,
                    "snippets_enabled": true,
                    "service_desk_enabled": false,
                    "service_desk_address": null,
                    "can_create_merge_request_in": true,
                    "issues_access_level": "enabled",
                    "repository_access_level": "enabled",
                    "merge_requests_access_level": "enabled",
                    "forking_access_level": "enabled",
                    "wiki_access_level": "enabled",
                    "builds_access_level": "enabled",
                    "snippets_access_level": "enabled",
                    "pages_access_level": "private",
                    "emails_disabled": null,
                    "shared_runners_enabled": true,
                    "lfs_enabled": true,
                    "creator_id": 1,
                    "import_status": "none",
                    "import_error": null,
                    "open_issues_count": 2,
                    "runners_token": "jJggsxmDvs_YNwfz5ZtV",
                    "ci_default_git_depth": 50,
                    "public_jobs": true,
                    "build_git_strategy": "fetch",
                    "build_timeout": 3600,
                    "auto_cancel_pending_pipelines": "enabled",
                    "build_coverage_regex": null,
                    "ci_config_path": null,
                    "shared_with_groups": [],
                    "only_allow_merge_if_pipeline_succeeds": true,
                    "allow_merge_on_skipped_pipeline": false,
                    "request_access_enabled": true,
                    "only_allow_merge_if_all_discussions_are_resolved": false,
                    "remove_source_branch_after_merge": true,
                    "printing_merge_request_link_enabled": true,
                    "merge_method": "merge",
                    "suggestion_commit_message": "",
                    "auto_devops_enabled": false,
                    "auto_devops_deploy_strategy": "continuous",
                    "autoclose_referenced_issues": true,
                    "permissions": {
                        "project_access": {
                            "access_level": 40,
                            "notification_level": 3
                        },
                        "group_access": null
                    }
                }
            }
        }
        ```

#### 请求权限 
项目管理员
#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|repository|int/str|远程仓库ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|请求成功|
|1|请求超时|

#### 响应数据

|字段|类型|说明|
|-|-|-|
|status|int|远程仓库返回的HTTP状态码|
|response|object|远程仓库返回的信息|



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
            "project": 2,
            "digest": true,
            "dev_id": [17],
            "limit": 604800,
            "sessionId": "b4RCDE7ovT5IMQrV2JnyC947JN8j8kD8"
        }
        ```
    === "响应 1"
        请求中 `digest=true` 时
        ```json
        {
            "code": 0,
            "data": [
                {
                    "mr_count": 4,
                    "commit_count": 3,
                    "additions": 27,
                    "deletions": 9,
                    "issue_count": 4,
                    "issue_times": [
                        522758,
                        593099,
                        2555,
                        21833
                    ],
                    "commit_times": [
                        1650339623,
                        1650343097,
                        1650343543
                    ],
                    "commit_lines" :[
                        10,
                        17,
                        9
                    ]
                    "mr_times": [
                        1650339623,
                        1650343097,
                        1650343543,
                        1650343643
                    ]
                }
            ]
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
                        "id": 784,
                        "merge_id": 153,
                        "title": "[SR.007.142] Exceptions in Issue List (#143)",
                        "description": "",
                        "repo": 2,
                        "state": "merged",
                        "url": "https://gitlab.secoder.net/undefined/frontend/-/merge_requests/153",
                        "authoredAt": 1652523915.308,
                        "reviewedAt": 1652524661.758,
                        "user_authored": 1,
                        "user_reviewed": 1
                    }
                ],
                "commits": [
                    {
                        "id": 162831,
                        "hash_id": "1295d142505610da63d81aca096c87214df94d9a",
                        "repo": 5,
                        "title": "[SR.007.140.I] catch router return fallback (#141)",
                        "createdAt": 1652502272.0,
                        "user_committer": 17,
                        "additions": 86,
                        "deletions": 79
                    }
                ],
                "issues": [
                    {
                        "id": 5831,
                        "issue_id": 141,
                        "repo": 5,
                        "title": "[SR.007.140] Bugfix: SRList",
                        "description": "",
                        "state": "closed",
                        "authoredAt": 1652496799.741,
                        "closedAt": 1652517983.375,
                        "url": "https://gitlab.secoder.net/undefined/frontend/-/issues/141",
                        "user_assignee": 17,
                        "user_authored": 17,
                        "user_closed": 1
                    }
                ],

            }
        }
        ```

#### 请求权限 
项目成员

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
|bug_issues|array\[objects\]|该迭代周期中所有被标记为bug的issue的详情，及其关联SR的相关信息|

## 远程代码

### 查看远程分支


用于从远程仓库请求分支情况。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/forward_branches/?project=2&repo=6&sessionId=6gr6q4MAo9l6jJxfl6s5SrK1Ukjx9g1A
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "http_status": 200,
                "body": [
                    {
                        "name": "dev",
                        "commit": {
                            "id": "exxxxxxxxxxxxxxxxxxxxxxxxxx",
                            "short_id": "exxxxxxx",
                            "created_at": "2022-05-14T10:33:03.000+00:00",
                            "parent_ids": null,
                            "title": "[SR.007.907.B] Merge: fix a page num incorrection (#102)",
                            "message": "[SR.007.907.B] Merge: fix a page num incorrection (#102)",
                            "author_name": "xxx",
                            "author_email": "xxx@secoder.net",
                            "authored_date": "2022-05-14T10:33:03.000+00:00",
                            "committer_name": "xxx",
                            "committer_email": "xxx@secoder.net",
                            "committed_date": "2022-05-14T10:33:03.000+00:00",
                            "web_url": "https://gitlab.secoder.net/xxxxxxxx"
                        },
                        "merged": false,
                        "protected": false,
                        "developers_can_push": false,
                        "developers_can_merge": false,
                        "can_push": true,
                        "default": true,
                        "web_url": "https://gitlab.secoder.net/xxx"
                    }
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
|repo|int/str|仓库ID|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为远程仓库不存在|
|2|查询失败，因为不支持这种类型的远程仓库|
|4|查询失败，因为向远程服务器请求超时|
|5|查询失败，因为向远程服务器请求的过程出现异常|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|http_status|int|远程仓库返回的http状态码|
|body|object|远程仓库返回内容的转发|


### 查看远程文件树


用于从远程仓库请求分支情况，以查看代码。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/forward_tree/?project=2&repo=6&ref=dev&path=/&sessionId=6gr6q4MAo9l6jJxfl6s5SrK1Ukjx9g1A
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "http_status": 200,
                "body": [
                    {
                        "id": "b9c5210aad2bda5d3b185e84d04e3be217b856e1",
                        "name": "backend",
                        "type": "tree",
                        "path": "backend",
                        "mode": "040000"
                    },
                    {
                        "id": "fa0c2d4c16b7e6057346c3606a7186a05b561276",
                        "name": "rdts",
                        "type": "tree",
                        "path": "rdts",
                        "mode": "040000"
                    }
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
|repo|int/str|仓库ID|
|path|str|要查找的地址|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为远程仓库不存在|
|2|查询失败，因为不支持这种类型的远程仓库|
|4|查询失败，因为向远程服务器请求超时|
|5|查询失败，因为向远程服务器请求的过程出现异常|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|http_status|int|远程仓库返回的http状态码|
|body|object|远程仓库返回内容的转发|

### 查看远程代码与SR关联

用于查看指定文件中代码块与相应 SR 和 commit 的关联。

??? example "示例"
    === "请求"
        ```bash
        curl https://example.com/rdts/forward_code_sr/?project=2&repo=3&ref=dev&path=pytest.ini&sessionId=RL0Waf4UdvMlXf48Gfvt415aMh4mUOSb
        ```
    === "响应"
        ```json
        {
            "code": 0,
            "data": {
                "relationship": [
                    {
                        "local_commit": 1119,
                        "remote_commit": null,
                        "lines": [
                            "[pytest]",
                            "DJANGO_SETTINGS_MODULE = backend.settings",
                            "python_files = tests.py test*.py *_tests.py"
                        ],
                        "SR": 125
                    }
                ],
                "SR": {
                    "125": {
                        "id": 125,
                        "project": 2,
                        "title": "SR.002.003",
                        "description": "配置后端 coverage 检测与 SonarQube",
                        "priority": 7,
                        "rank": 1,
                        "state": "Done",
                        "createdBy": 1,
                        "createdAt": 1650993749.159988,
                        "disabled": false
                    }
                },
                "Commits": {
                    "xxxxxxxxxxxxxx": {
                        "id": 1119,
                        "hash_id": "xxxxxxxxxxxxxx",
                        "repo": 1,
                        "title": "[SR.002.003] Finish sub-branch testing",
                        "message": "[SR.002.003] Finish sub-branch testing\n",
                        "commiter_email": "xxxxxxxxxxxxxx@secoder.net",
                        "commiter_name": "xxxxxxxxxxxxxx",
                        "createdAt": 1647399192.0,
                        "url": "https://gitlab.secoder.net/xxxxxxxxxxxxxx",
                        "commite_date": 0.0,
                        "user_committer": 1,
                        "additions": 3,
                        "deletions": 14
                    }
                }
            }
        }
        ```

#### 请求权限 
项目成员

#### 请求参数
|参数|类型|说明|
|-|-|-|
|project|int/str|项目ID|
|repo|int/str|仓库ID|
|path|str|要查找文件的地址|

#### 响应状态
|状态码|说明|
|-|-|
|0|查询成功|
|1|查询失败，因为远程仓库不存在|
|2|查询失败，因为不支持这种类型的远程仓库|
|4|查询失败，因为向远程服务器请求超时|
|5|查询失败，因为向远程服务器请求的过程出现异常|

#### 响应数据
|字段|类型|说明|
|-|-|-|
|relationship|array|代码块及从代码块到 SR 和 commit 的映射关系|
|SR|array|代码块中涉及的 SR 信息|
|Commits|array|代码块中涉及的 commit 信息|

