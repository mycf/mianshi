
| 层次名  | 作用                                                       |
| ------- | ---------------------------------------------------------- |
| Service | 业务层。包括业务代码的接口与实现，即开发者实现的业务代码。 |
| config  | 配置层。主要围绕ServiceConfig（暴露的服务配置）和ReferenceConfig（引用的服务配置）两个实现类展开，初始化配置信息，可以理解该层管理了整个Dubbo的配置。                                                     |
| proxy        | 服务代理层。在Dubbo中，无论生产者还是消费者，框架都会生成一个代理类，整个过程                                                           |
|         |                                                            |
# 负载均衡

![img](https://img-blog.csdnimg.cn/20210508201651489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkzNDYwNw==,size_16,color_FFFFFF,t_70)

