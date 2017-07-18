# CenterHub
1. 基于 SignalR
1. 使用此 Hub 进行统一的数据修改推送
1. 使用 WebApi 进行统一的数据操作、获取和提交
1. 可以连接 EF 的 SaveChanges 在进行数据修改时进行处理
1. 默认使用 Redis 处理消息队列

## Publish
``` c#
//Register(typeof(Task),Id,ProductId,XType.Modified,Properties);
//Register(typeof(Task),task,XType.Modified,Properties);
Publish(entity);// in ef
```
## Subscribe in Hub 

方法参数

```C#
Subscribe(typeof(Task),XType.Created,"clientMethodName");
Subscribe(typeof(Task),XType.Modified,"clientMethodName");
Subscribe(typeof(Task),
                XType.Modified,
                "clientMethodName",
                c=>c.Property1,c=>c.Property2);
```
方法种类
```c#
Subscribe**Duplicate** // one message one push
Subscribe // batch message one push


v2.0
SubscribePersistence or not // if no client active not push while client well
```
## XType Desgin
``` yml
XType
    - Created
    - Modified
    - Deleted
    - \\ SpicalPropertyModified
```

