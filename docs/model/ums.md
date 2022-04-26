<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# UMS 模型 


## 项目模型 Project

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|项目ID|
|title |Char |索引|项目名称，最长255字符|
|description |Text ||项目描述|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |项目创建时间，默认创建当时的时间戳|
|avatar |Text| |项目图标，base64编码保存|
|remote_sr_pattern_extract|Text||用于匹配SR标题的正则表达式|
|remote_issue_iid_extract|Text||用于匹配issue编号的正则表达式|



## 用户模型 User

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|用户ID|
|name |Char |索引|用户名，最长255字符|
|email |Char |索引|用户邮箱，最长255字符|
|password |Text ||用户密码，储存哈希后的结果|
|avatar |Text| |用户头像，base64编码保存|
|disabled |Boolean ||是否删除，默认为False|
|createdAt |Float| |用户创建时间，默认创建当时的时间戳|
|project |ManyToMany([Project](#project))| |用户所在的项目，通过UserProjectAssociation模型实现|
|email_verified |Boolean ||email是否已经验证，默认为False|


## 会话模型 SessionPool
|字段|类型|属性|说明|
|-|-|-|-|
|sessionId |Char|索引 |会话哈希值|
|user |ForeignKey([User](#user))||会话对应的用户|
|expireAt |DateTime| |会话过期时间，默认48小时后过期|



## 用户-项目关联模型 UserProjectAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|user |ForeignKey([User](#user))|索引|对应用户|
|project |ForeignKey([Project](#project))|索引|对应项目|
|role |Text ||用户在项目中的角色|

其中，联合索引 (user, project)


## 项目-邀请码关联模型 ProjectInvitationAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|project |ForeignKey([Project](#project))|索引|对应项目|
|invitation |Char|索引 |项目对应的邀请码，最长64字符|
|role |Text ||获得邀请码后用户成为的角色|


## 邮件重置密码模型 PendingModifyPasswordEmail
|字段|类型|属性|说明|
|-|-|-|-|
|hash1 |Char |索引， 唯一|邮件中发送的哈希值，最长100字符|
|hash2 |Char |索引|修改密码会话的哈希值，最长100字符|
|user |ForeignKey([User](#user))||修改密码的用户|
|email |Text ||用于修改密码的邮箱|
|createdAt |Float ||申请修改密码的时间，默认为创建时的时间戳|
|beginAt |Float| |首次点击链接的时间，默认为-1|
|hash1_verified |Boolean ||用户是否已经点击了链接，默认为False|


## 邮箱验证模型 PendingVerifyEmail
|字段|类型|属性|说明|
|-|-|-|-|
|hash |Char |索引， 唯一|邮件中发送的哈希值，最长100字符|
|user |ForeignKey([User](#user))||验证邮箱的用户|
|email |Text ||待验证的邮箱|
|createdAt |Float ||申请验证邮箱的时间，默认为创建时的时间戳|
|is_major |Boolean ||验证的是否是主邮箱|


## 用户-副邮箱关联模型 UserMinorEmailAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|user |ForeignKey([User](#user))|索引|对应用户|
|email |Char |索引|最长255字符|
|verified |Boolean ||该邮箱是否已经验证，默认为False|

## 用户-远程用户名关联模型 UserRemoteUsernameAssociation
|字段|类型|属性|说明|
|-|-|-|-|
|user|ForeignKey([User](#user))|索|本地用户|
|repository|ForeignKey([Repository](../rdts/#Repository))||仓库|
|remote_name|Char||用户在远程仓库中的名称，最长255字符|

其中，联合索引 (remote_name, repository)

## 配置模型 Config



|字段|类型|属性|说明|
|-|-|-|-|
|key|Char|索引|配置键，最长255字符|
|value|Text||配置值|

!!! info "配置的初始化"
    该模型主要用于保存一些在运行过程中可能随时改变的数据，如SMTP发信服务配置、修改密码邮件的模板等等。

    在 `ums` 模块被加载，且相关数据表已经创建后
    
    - 若数据库中已经存在该键，则值维持不变
    - 若数据库中不存在该键，则将键-值对的默认值添加到数据库中。
    


