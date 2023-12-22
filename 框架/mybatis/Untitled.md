# 缓存

```plantuml
@startuml
skin rose
participant Actor
Actor -> SqlSessionTemplate : selectList
activate SqlSessionTemplate
SqlSessionTemplate -> DefaultSqlSession : selectList
activate DefaultSqlSession
' DefaultSqlSession -> Configuration : getMappedStatement
' activate Configuration
' Configuration --> DefaultSqlSession
' deactivate Configuration
' DefaultSqlSession -> DefaultSqlSession : wrapCollection
' activate DefaultSqlSession
' DefaultSqlSession --> DefaultSqlSession
' deactivate DefaultSqlSession
DefaultSqlSession -> CachingExecutor : query
activate CachingExecutor
alt 开启二级缓存
CachingExecutor -> TransactionalCacheManager : getObject
note right
查询二级缓存
end note
activate TransactionalCacheManager
TransactionalCacheManager --> CachingExecutor
deactivate TransactionalCacheManager
end
alt 二级缓存未查到
CachingExecutor -> BaseExecutor : query
activate BaseExecutor
BaseExecutor -> PerpetualCache : getObject
activate PerpetualCache
note right
一级缓存
end note
PerpetualCache --> BaseExecutor
deactivate PerpetualCache
BaseExecutor -> BaseExecutor : queryFromDatabase
BaseExecutor -> CachingExecutor : 返回数据库结果
deactivate BaseExecutor
end
CachingExecutor --> DefaultSqlSession
deactivate CachingExecutor
return
@enduml
```

## 禁用二级缓存
`@Options(useCache = false)`
```java
@Select("select * from ht")
@Options(useCache = false)
List<Map> select();

```
## 在debug代码时发现明明是一个接口生成代理类，为什么打印的是一个实现类。
![image.png](https://gitee.com/ycfan/images/raw/master/img/20231220104059.png)

- IDEA在debug程序时，当debug到某个对象时，会调用对象的toString()方法，用来在debug界面显示对象信息。
- IDEA调用toString()方法时，即使在toString()方法中设置了断点，该断点也不会被触发，也就是说，开发者多数情况下不会知道toString()方法被调用了
- 多数情况下调用一下toString()方法没有什么问题，但是也有例外，比如重写了toString()方法的类，随意的调用toString()方法会导致未知的问题。

可以看到我们代理类的 `toString()` 方法调用的时候，会自动获取目标对象`DefaultSqlSession`，所以debug的时候看到`DefaultSqlSession`
![image.png](https://gitee.com/ycfan/images/raw/master/img/20231220104800.png)

## 解决办法
我们把自动`toString()`关闭就好了 
![image.png](https://gitee.com/ycfan/images/raw/master/img/20231220104554.png)
再次运行发现正常了，`toString()`需要自己点击了
![image.png](https://gitee.com/ycfan/images/raw/master/img/20231220104213.png)
