<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>
# RMS 模型 


## 迭代模型 Iteration

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|迭代ID|
|project|ForeignKey([Project](../ums/#project))|索引|迭代周期的归属|
|sid|Int|||
|title |Char||项目名称，最长255字符|
|begin|Float||迭代的开始时间|
|end|Float||迭代的完毕时间|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |迭代创建时间，默认创建当时的时间戳|

## 用户-迭代关联模型 UserIterationAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|user |ForeignKey([User](../user/#user))|索引|对应的用户|
|iteration|ForeignKey([Iteration](#iteration))|索引|对应的迭代|

其中，联合索引 (user, iteration)

其中，联合唯一 (user, iteration)


## 原始需求模型 IR

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|IRID|
|project|ForeignKey([Project](../ums/#project))|索引|原始需求的归属|
|title |Char |索引|原始需求名称，最长255字符|
|description |Text ||原始需求描述|
|rank|Int||原始需求的序|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |原始需求创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](#user))||创建的用户|

其中，联合索引(title,project)

## 功能需求模型 SR

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|IRID|
|project|ForeignKey([Project](../ums/#project))|索引|功能需求的归属|
|title |Char |索引|功能需求名称，最长255字符|
|description |Text ||功能需求描述|
|priority|Int ||功能需求的优先级|
|rank|Int||功能需求的优先级|
|IR|ManyToMany||关联的IR|
|state|Text|Enum (TODO, WIP, Reviewing, Done)|功能需求的状态，共有四种|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |功能需求创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](../user/#user))||创建的用户|

其中，联合索引(title,project)

## 原始需求-功能需求关联模型 IRSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|IR|ForeignKey([IR](#ir))|索引|对应的 IR|
|SR|ForeignKey([SR](#sr))|索引|对应的 SR|

其中，联合唯一(ir,sr)

## 功能需求-迭代关联模型 SRIteratiionAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([IR](#ir))|索引|对应的 SR|
|iteration|ForeignKey([Iteration](#iteration))|索引|对应的 Iteration|

其中，联合唯一(iteration,SR)

## 服务模型 Service

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|服务的ID|
|project|ForeignKey([Project](../ums/#project))|索引|服务的归属|
|title |Char |索引|服务名称，最长255字符|
|description |Text ||服务描述|
|rank|Int||服务的序|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |服务创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](../user/#user))||创建的用户|

其中，联合索引(title,project)

## SR 变更记录模型 SR_Changelog

|字段|类型|属性|说明|
|-|-|-|-|
|id|BigAuto|主键|记录的 ID|
|project|ForeignKey([Project](../ums/#project))||  变更记录的归属|
|SR|ForeignKey([SR](#sr))||归属的 SR|
|description|Text||变更的描述|
|formerState|Text|Enum(TODO, WIP, Done, Reviewing)|变更前的状态|
|formerDescription|Text||变更之前 SR 的描述|
|changedBy|ForeignKey([User](../user/#user))||更改 SR 的用户|
|changedAt|Float||修改时间，默认修改当时的时间戳|


## 服务-功能需求关联模型 ServiceSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](#sr))||归属的 SR|
|service|ForeignKey([Service](#service))||归属的  Service|

其中，联合唯一(SR,service)

## 原始需求-迭代关联模型 IRIterationAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|IR|ForeignKey([IR](#ir))||归属的 IR|
|iteration|ForeignKey([Iteration](#iteration))||归属的迭代|

其中，联合唯一(IR,iteration)

## 项目-迭代关联模型 ProjectIterationAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|project|ForeignKey([Project](../ums/#project))||归属的项目|
|iteration|ForeignKey([Iteration](#iteration))||该项目所处于的迭代|

其中，联合唯一(project,iteration)

## 用户-功能需求关联模型 ProjectIterationAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|SR|ForeignKey([SR](#sr))||归属的 SR|
|user|ForeignKey([User](../user/#user))|唯一|主管该 SR 的用户|