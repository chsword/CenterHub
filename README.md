# CenterHub
1. 基于 SignalR
1. 使用此 Hub 进行统一的数据修改推送
1. 使用 WebApi 进行统一的数据操作、获取和提交
1. 可以连接 EF 的 SaveChanges 在进行数据修改时进行处理
1. 默认使用 Redis 处理消息队列

## 服务器端实现

### Listener
处理 Queue 中的信息
成功取出队列 -> 查询队列中的 Method 对象 -> 获取指定的 ClientGroup -> 判断是否推送理改产生的 Model -> 判断是否满足推送条件

```
问题1：ClientGroup 的组名需要自定义（系统注册时定义规则）
问题2：
```

### Publish
``` c#
//publish info to queue
//Publish(typeof(Task),Id,ProductId,XType.Modified,Properties);
//Publish(typeof(Task),task,XType.Modified,Properties);
Publish(entity);// in ef 
```
### Register in Hub 

方法参数

```C#
Register(typeof(Task),XType.Created,"clientMethodName");
Register(typeof(Task),XType.Modified,"clientMethodName");
Register(typeof(Task),
                XType.Modified,
                "clientMethodName",
                (x)=>{
                  return x["UserId"]==3;
                });
```
方法种类
```c#
Subscribe**Duplicate** // one message one push
Subscribe // batch message one push


v2.0
SubscribePersistence or not // if no client active not push while client well
```
### XType Desgin
``` yml
XType
    - Created
    - Modified
    - Deleted
    - \\ SpicalPropertyModified
```
## 客户端实现




### 问题及解决方案
1. 如何解决使用字符串调用方法的问题
``` C#
var client=((IClientProxy)GlobalHost.ConnectionManager.GetHubContext<TransportHub>()
                                .Clients.All);
client.Invoke(method, arg1, arg2);
```
2. 如何解决服务器端得知客户端方法的问题
```
暂考虑分组问题
```
