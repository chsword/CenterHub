# CenterHub
1. 使用此 Hub 进行统一的数据修改推送
2. 使用 WebApi 进行统一的数据操作、获取和提交


## 方法设计
``` c#
//Register(typeof(Task),Id,ProductId,XType.Modified,Properties);
//Register(typeof(Task),task,XType.Modified,Properties);
Register(entity);
```
## Register in Hub 
```C#
RegisterInHub(typeof(Task),XType.Created,"clientMethodName");
RegisterInHub(typeof(Task),XType.Modified,"clientMethodName");
RegisterInHub(typeof(Task),
                XType.Modified,
                "clientMethodName",
                c=>c.Property1,c=>c.Property2);

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

