# CenterHub
1. 基于 SignalR
1. 使用此 Hub 进行统一的数据修改推送
1. 使用 WebApi 进行统一的数据操作、获取和提交
1. 可以连接 EF 的 SaveChanges 在进行数据修改时进行处理
1. 默认使用 Redis 处理消息队列


## 方法设计
``` c#
//Register(typeof(Task),Id,ProductId,XType.Modified,Properties);
//Register(typeof(Task),task,XType.Modified,Properties);
Register(entity);
```
## Register in Hub 

方法参数

```C#
RegisterInHub(typeof(Task),XType.Created,"clientMethodName");
RegisterInHub(typeof(Task),XType.Modified,"clientMethodName");
RegisterInHub(typeof(Task),
                XType.Modified,
                "clientMethodName",
                c=>c.Property1,c=>c.Property2);
```
方法种类
```c#
RegisterInHub**Duplicate** // one message one push
RegisterInHub // batch message one push


v2.0
Persistence or not // if no client active not push while client well
```
## XType Design
``` yml
XType
    - Created
    - Modified
    - Deleted
    - \\ SpicalPropertyModified
```

