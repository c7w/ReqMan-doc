# 数据库模型

!!! info "字段类型说明"
    由于使用了Django的ORM模型，一些数据类型的描述将与MySQL不同，部分对应关系列举如下。
    
    |ModelField|MySQL Type|
    |-|-|
    |ForeignKey | `int`|
    |Boolean | `tinyint`|
    |Float |`double`|
    |Text|`longtext`|
    |BigAuto|`bigint`|
    |Char|`varchar`|
    |ManyToMany |通过中间表实现，数据库中没有相应的字段|

    没有指定主键的模型会使用自动生成的 `bigint` 主键区分记录。