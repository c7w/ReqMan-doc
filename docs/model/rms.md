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
|project|ForeignKey([Project](#project))|索引|迭代周期的归属|
|sid|Int|||
|title |Char||项目名称，最长255字符|
|begin|Float||迭代的开始时间|
|end|Float||迭代的完毕时间|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |迭代创建时间，默认创建当时的时间戳|

## 用户-迭代关联模型 UserIterationAssociation

|user |ForeignKey([User](#user))|索引|对应用户|
|iteration|ForeignKey([Iteration](#iteration))|索引|对应的迭代|

其中，联合索引 (user, iteration)

其中，联合唯一 (user, iteration)


## 原始需求模型 IR

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|IRID|
|project|ForeignKey([Project](#project))|索引|  IR的归属|
|title |Char |索引|IR名称，最长255字符|
|description |Text ||IR描述|
|rank|Int||IR 的优先级|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |IR 创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](#user))||创建的用户|

其中，联合索引(title,project)

## 功能需求模型 SR

|字段|类型|属性|说明|
|-|-|-|-|
|id |BigAuto |主键|IRID|
|project|ForeignKey([Project](#project))|索引|  SR的归属|
|title |Char |索引|SR名称，最长255字符|
|description |Text ||SR描述|
|priority|Int ||SR 的优先级|
|rank|Int||SR 的优先级|
|IR|多对多关系||关联的IR|
|state|enum (TODO,WIP,Reviewing,Done)||SR 的状态，共有四种|
|disabled |Boolean| |是否删除，默认为False|
|createdAt |Float| |SR 创建时间，默认创建当时的时间戳|
|createBy|ForeignKey([User](#user))||创建的用户|

其中，联合索引(title,project)

## 原始需求-功能需求关联模型 IRSRAssociation

|字段|类型|属性|说明|
|-|-|-|-|
|IR|ForeignKey([IR](#ir))|索引|对应的 IR|
|SR|ForeignKey([IR](#ir))|索引|对应的 SR|

其中，联合索引(ir,sr)