# CenterHub
- [x] 1. 基于 SignalR
- [x] 1. 使用此 Hub 进行统一的数据修改推送
- [ ] 1. 使用 WebApi 进行统一的数据操作、获取和提交
- [x] 1. 可以连接 EF 的 SaveChanges 在进行数据修改时进行处理
- [ ] 1. 默认使用 Redis 处理消息队列

## 服务器端实现

### Listener
处理 Queue 中的信息
成功取出队列 -> 查询队列中的 Method 对象 -> 获取指定的 ClientGroup -> 判断是否推送理改产生的 Model -> 判断是否满足推送条件


- [ ] 1. 针对一系统多项目
  1. ClientGroup 的组名需要自定义（系统注册时定义规则）


### Publish
- [x] 支持 EF 直接在 SaveChanges 时进行通知
``` c#
//publish info to queue
//Publish(typeof(Task),Id,ProductId,XType.Modified,Properties);
//Publish(typeof(Task),task,XType.Modified,Properties);
Publish(entity);// in ef 
```
### Register in Hub 

方法参数


- [x] Register(typeof(Task), XType.Created, "clientMethodName");
- [x] Register(typeof(Task), XType.Modified | XType.Created, "clientMethodName");
- [x] Register(typeof(Task), XType.Modified, "clientMethodName", (d) => d["UserId"].Equals(3));

- [ ] 区分是否进行批量推送
- [ ] Register**Duplicate** // one message one push
- [ ] Register // batch message one push


v2.0
- [ ] RegisterPersistence or not // if no client active not push while client well

### XType Desgin
- [x] 完成操作类型检测
``` yml
XType
    - Created
    - Modified
    - Deleted
    - \\ SpicalPropertyModified
```
## 客户端实现

- [ ] 1. 零散的注册方法
- [ ] 2. 收集零散的注册方法
- [x] 3. 统一的 SignalR 启动方式



### 问题及解决方案
- [x] 1. 如何解决使用字符串调用方法的问题
``` C#
var client=((IClientProxy)GlobalHost.ConnectionManager.GetHubContext<CenterHub>()
                                .Clients.All);
client.Invoke(method, arg1, arg2);
```
- [x] 2. 如何解决服务器端得知客户端方法的问题

~~~暂考虑分组问题~~~
使用双向注册 服务器端注册一次，客户端注册一次，双向注册匹配的才进行推送




