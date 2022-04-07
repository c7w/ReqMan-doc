<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>
# RDTS 模型

## 仓库模型 Repository

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|仓库ID|
|url|Text||仓库的链接，最大长度 255 字符|
|project|ForeignKey([Project](../ums/#project))|索引|仓库的归属项目|
|title |Text|索引|仓库名称，最长255字符|
|description |Text ||仓库描述|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |仓库创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](../ums/#user))||创建的用户|

其中，联合索引(project,title)

## 提交模型 Commit

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|提交ID|
|hash_id|Text||提交记录的 hash 值，最大长度 255 字符|
|repo|ForeignKey([Repository](#repository))|索引|提交记录归属的仓库|
|title |Text||提交的名称，最长255字符|
|message|Text||提交的信息|
|commiter_email|Text|索引|提交者的邮件，最大长度 255 字符|
|commiter_name|Text|索引|提交者的姓名，最大长度 255 字符|
|createAt|Float||提交记录的创建时间|
|url|Text||提交记录的链接|
|disabled |Boolean| |是否删除，默认为False|

## 合并请求模型 MergeRequest

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|合并请求 ID|
|merge_id|Int||合并的 ID|
|repo|ForeignKey([Repository](#repository))|索引|合并请求归属的仓库|
|title |Text||合并请求的名称，最长255字符|
|description |Text ||合并请求的描述|
|state|Text|Enum(mergerd,closed,openeds)|合并请求的状态，共有三种|
|authoredByEmail|Text||提请者的邮箱，最大长度 255 字符，默认为空|
|authoredByUserName|Text|索引|提请者的用户名，最大长度 255 字符，默认为空|
|authoredAt|Float||提请时间，可为空|
|reviewedByEmail|Text||复盘者的邮箱，最大长度 255 字符，默认为空|
|reviewedByUserName|Text|索引|复盘者的用户名，最大长度 255 字符，默认为空|
|reviewedAt|Float||复盘时间，可为空|
|url|Text||合并请求的链接|
|disabled |Boolean| |是否删除，默认为False|

## 议题模型 Issue

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|议题 ID|
|issue_id|Int||议题 ID|
|repo|ForeignKey([Repository](#repository))|索引|议题归属的仓库|
|title |Text||议题的名称，最长255字符|
|description |Text ||议题的描述|
|state|Text|Enum(closed,opened)|议题的状态，共有两种|
|authoredByUserName|Text|索引|提出者的用户名，最大长度 255 字符，默认为空|
|authoredAt|Float||提出时间，可为空|
|updatedAt|Float||更新时间，可为空|
|closedByUserName|Text|索引|关闭者的用户名，最大长度 255 字符，默认为空|
|closedAt|Float||关闭时间，可为空|
|assigneeUserName|Text||委托者的用户名，最大长度 255 字符，默认为空|
|url|Text||议题的链接|
|disabled |Boolean| |是否删除，默认为False|
|labals|Text||议题的标签，使用 json 格式存储|
|is_bug|Boolean||是否是缺陷相关议题，默认为 False|

## 提交记录-功能需求关联模型 CommitSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|commit|ForeignKey([Commit](#提交模型-commit))||对应的提交记录|

其中，联合唯一(SR,commit)

## 合并请求-功能需求关联模型 MRSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|MR|ForeignKey([MergeRequest](#合并请求模型-mergerequest))||对应的 MergeRequest|

其中，联合唯一(MR,SR)

## 议题-功能需求关联模型 IssueSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|issue|ForeignKey([Issue](#议题模型-issue))||对应的议题|

其中，联合唯一(SR,issue)

## 议题-合并请求关联模型 IssueMRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|issue|ForeignKey([Issue](#议题模型-issue))||对应的议题|
|MR|ForeignKey([MergeRequest](#合并请求模型-mergerequest))||对应的 MergeRequest|

其中，联合唯一(issue,MR)