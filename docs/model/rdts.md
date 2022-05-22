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
|url|Char|索引|表示仓库的URL，最长255字符|
|project|ForeignKey([Project](../ums/#project))|索引|仓库的归属项目|
|title |Text|索引|仓库名称，最长255字符|
|description |Text ||仓库描述|
|disabled |Boolean| |是否已删除，默认为False|
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
|commite_date |Float||提交记录最后一次同步的时间|
|url|Text||提交记录的链接|
|disabled |Boolean| |是否已删除，默认为False|
|user_commiter|ForeignKey([User](../ums/#user))||关联提交者对应的本地用户|
|additions|Integer||本次提交的增加行数|
|deletions|Integer||本次提交的删除行数|


## 合并请求模型 MergeRequest

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|合并请求 ID|
|merge_id|Integer||合并的 ID|
|repo|ForeignKey([Repository](#repository))|索引|合并请求归属的仓库|
|title |Text||合并请求的名称，最长255字符|
|description |Text ||合并请求的描述|
|state|Text|Enum(merged,closed,opened)|合并请求的状态，共有三种|
|authoredByEmail|Text||提请者的邮箱，最大长度 255 字符，默认为空|
|authoredByUserName|Text|索引|提请者的用户名，最大长度 255 字符，默认为空|
|authoredAt|Float||提请时间，可为空|
|reviewedByEmail|Text||复盘者的邮箱，最大长度 255 字符，默认为空|
|reviewedByUserName|Text|索引|复盘者的用户名，最大长度 255 字符，默认为空|
|reviewedAt|Float||复盘时间，可为空|
|url|Text||合并请求的链接|
|disabled |Boolean| |是否已删除，默认为False|
|updated_at|Float| |最后一次同步时间|
|user_authored|ForeignKey([User](../ums/#user))||关联提出请求者对应的本地用户|
|user_reviewed|ForeignKey([User](../ums/#user))||关联合并请求者对应的本地用户|

## 议题模型 Issue

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键||
|issue_id|Integer||议题在相应仓库中的唯一ID|
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
|disabled |Boolean| |是否已删除，默认为False|
|labals|Text||议题的标签，使用 json 格式存储|
|is_bug|Boolean||是否是缺陷相关议题，默认为 False|
|user_assignee |ForeignKey([User](../ums/#user))||关联Issue的指派者对应的本地用户|
|user_authored|ForeignKey([User](../ums/#user))||关联Issue的提出者对应的本地用户|
|user_closed|ForeignKey([User](../ums/#user))||关联Issue的关闭者对应的本地用户|

## 提交记录-功能需求关联模型 CommitSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|commit|ForeignKey([Commit](#commit))||对应的提交记录|
|auto_added|Bool||是否是自动添加的|

其中，联合唯一(SR,commit)

## 合并请求-功能需求关联模型 MRSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|MR|ForeignKey([MergeRequest](#mergerequest))||对应的 MergeRequest|
|auto_added|Bool||是否是自动添加的|

其中，联合唯一(MR,SR)

## 议题-功能需求关联模型 IssueSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](../rms/#sr))||对应的 SR|
|issue|ForeignKey([Issue](#issue))||对应的议题|
|auto_added|Bool||是否是自动添加的|

其中，联合唯一(SR,issue)

## 议题-合并请求关联模型 IssueMRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|issue|ForeignKey([Issue](#issue))||对应的议题|
|MR|ForeignKey([MergeRequest](#mergerequest))||对应的 MergeRequest|
|auto_added|Bool||是否是自动添加的|

其中，联合唯一(issue,MR)

## 远程仓库模型 RemoteRepo
|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|远程仓库ID|
|type|Char||表示远程仓库的类型，目前只支持 `gitlab`，最长50字符|
|remote_id|Text||远程仓库的标识符|
|access_token|Text||用于访问远程仓库的访问令牌|
|enable_crawling|Boolean||是否定时同步该仓库|
|info|Text||用于连接远程仓库所需的额外信息，如果网址等，用于实现对不同远程仓库的对接|
|repo|ForeignKey([Repository](#repository))|索引|表示远程仓库对应的仓库|
|secret_token|Char|索引，唯一|用于webhook匹配本地仓库，最长255字节|
|created |Boolean||表示仓库创建后是否已经完成了首次同步|

## 同步记录模型 Crawllog
|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|同步记录 ID|
|repo|ForeignKey([RemoteRepo](#remoterepo))||爬取对应的远程仓库|
|time|Float||同步开始时的时间戳|
|status|Integer||同步过程中远程响应的HTTP状态码|
|message|Text||同步过程中远程响应的错误信息|
|request_type|Text||同步的类型：commit, merge, issue; general为尚未开始同步就出错|
|finished|Boolean||同步是否成功结束：False 表示处理过程中出错，没有处理完|
|updated|Boolean||本次同步是否对数据库做了修改|
|is_webhook|Boolean||本次同步是否是 Webhook 发起的|

## WebHook等待列表模型 PendingWebhookRequests
|字段|类型|属性|说明|
|-|-|-|-|
|remote|ForeignKey([RemoteRepo](#remoterepo))||该WebHook请求对应的RemoteRepo|
|body|Text||WebHook请求的请求体|

## Commit更新记录模型 CommitCrawlAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|commit|ForeignKey([Commit](#commit))||对应的Commit|
|crawl|ForeignKey([CrawlLog](#crawllog))||关联的同步记录|
|operation|Char||操作类型：`update`, `insert`|

## MergeRequest更新记录模型 MergeCrawlAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|merge|ForeignKey([MergeRequest](#mergerequest))||对应的MergeRequest|
|crawl|ForeignKey([CrawlLog](#crawllog))||关联的同步记录|
|operation|Char||操作类型：`update`, `insert`|


## Issue更新记录模型 IssueCrawlAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|issue|ForeignKey([Issue](#issue))||对应的Issue|
|crawl|ForeignKey([CrawlLog](#crawllog))||关联的同步记录|
|operation|Char||操作类型：`update`, `insert`|
