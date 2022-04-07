# UMS模块API文档
## 权限约定
考虑到接口中有大量重复鉴权操作，使用`Restframework`进行统一的鉴权操作。这些鉴权操作将在视图函数前进行，产生固定状态码。

|JSON状态码|HTTP状态码|异常类型|说明|
|-|-|-|-|
|-4|403|AuthenticationFailed|请求参数中不含`sessionId`|
|-3|429|Throttled|用户访问过于频繁|
|-2|403|PermissionDenied|用于请求的sessionId权限不足|
|-1|400|ParamErr|请求参数错误，若`DEBUG=True`，则会显示参数错误的具体信息|
|-100|-|其他 APIException| - |

后续API文档中不对这些状态码单独说明。

!!! info "部分鉴权功能尚未启用"
    由于代理设置，我们当前无法获得访问者的IP地址，访问频率控制功能没有启用。