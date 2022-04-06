<style>
    .md-nav--secondary>ul>li>nav>ul>li>nav {
        display: none;
    }
</style>

# RMS 模块模型接口
!!! warning 
    不加特殊说明，所有请求都需要有 `sessionId` 字段。

!!! note "统一接口"
    有关 RMS 中模型的接口全部通过以下接口进行访问和操作，其中，查询操作通过 GET 请求，而创建、修改以及删除操作通过 POST 请求。

<div class="grid cards" markdown>
- <span>**[ GET ]** &nbsp;&nbsp; /rms/project/</span>
</div>

<div class="grid cards" markdown>
- <span>**[ POST ]** &nbsp;&nbsp; /rms/project/</span>
</div>