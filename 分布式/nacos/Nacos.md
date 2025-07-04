这里是官方的书籍
[[nacos.pdf]]
https://developer.aliyun.com/ebook/36?spm=a2c6h.26392470.ebook-read.55.6da71513bjwFLU
[官网]: https://nacos.io/
Dynamic Naming and Configuration Service
# 简介
更易于构建云原生应用的**动态服务发现**、**配置管理**和**服务管理**平台。


## 优势
**易用**：简单的数据模型，标准的restfuApi，易用的控制台，丰富的使用文档
**稳定**：99.9%高可用
**实时**：
**规模**：
# Nacos 配置模型

## 命名空间(Namespace)
用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data lD 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源(如数据库配置、限流阈值、降级开关)隔离等。如果在没有指定 Namespace 的情况下，默认使用 public 命名空间。

## 配置组(Group)
Nacos 中的一组配置集，是配置的维度之一。通过一个有意义的字符串(如 ABTest 中的实验组、对照组)对配置集进行分组，从而区分 Data lD 相同的配置集。当您在 Nacos 上创建一个配置时,如果未填写配置分组的名称，则配置分组的名称默认采用 ==DEFAULT_GROUP== 。配置分组的常见场景:不同的应用或组件使用了相同的配置项，如 database_url 配置和 MQ_Topic 配置。

## 配置ID(Data lD)
Nacos 中的某个配置集的 ID。配置集 ID 是划分配置的维度之一。Data lD 通常用于划分系统的配置集。一个系统或者应用可以包含多个配置集，每个配置集都可以被一个有意义的名称标识。DataID 尽量保障全局唯一，可以参考 Nacos Spring Cloud 中的命名规则:
```
${prefix}-${spring.profiles.active}-${file-extension}
```

## 下载
[下载地址](https://github.com/alibaba/nacos/releases)
```bash
unzip nacos-server-1.0.0.zip
cd nacos/bin
```
## 启动服务
```sh
sh startup.sh -m standalone
```
![[Nacos 2024-01-03 10.28.12.excalidraw|100%]]
## 打开控制台
```
http://192.168.1.5:8848/nacos/index.html
```
# Nacos 一致性协议

## 从服务注册来看

服务发现注册中心，在当前微服务体系下，是十分重要的组件，服务之间感知对方服务的当前可正常提供服务的实例信息，必须从服务发现注册中心进行获取，因此对于服务注册发现中心组件的可用性，提出了很高的要求，需要在任何场景下，尽最大可能保证服务注册发现能力可以对外提供服务，同时 Nacos 的服务注册发现设计，采取了心跳可自动完成服务数据补偿的机制。如果数据丢失的话，是可以通过该机制快速弥补数据丢失。

因此，**为了满足服务发现注册中心的可用性**，强一致性的共识算法这里就不太合适了，因为强一致性共识算法能否对外提供服务是有要求的，如果当前集群可用的节点数没有过半的话，整个算法直接“罢工”，而最终一致共识算法的话，更多保障服务的可用性，并且能够保证在一定的时间内各个节点之间的数据能够达成一致。

上述的都是针对于 Nacos 服务发现注册中的**非持久化服务**而言 (即需要客户端上报心跳进行服务实例续约)。而对于 Nacos 服务发现注册中的持久化服务，因为所有的数据都是直接使用调用 Nacos服务端直接创建，因此需要由 Nacos 保障数据在各个节点之间的强一致性，故而针对此类型的服务数据，选择了强一致性共识算法来保障数据的一致性。

## 从配置管理来看

配置数据，是直接在 Nacos 服务端进行创建并进行管理的，必须保证大部分的节点都保存了此配置数据才能认为配置被成功保存了，否则就会丢失配置的变更，如果出现这种情况，问题是很严重的，如果是发布重要配置变更出现了丢失变更动作的情况，那多半就要引起严重的现网故障了，因此**对于配置数据的管理，是必须要求集群中大部分的节点是强一致的**，而这里的话只能使用强一致性共识算法。


# Nacos 服务发现
Nacos 1.0.0 介绍的另外一个新特性是: 临时实例和持久化实例。在定义上区分临时实例和持久化实例的关键是健康检查的方式。**临时实例使用客户端上报模式**，而**持久化实例使用服务端反向探测模式**。临时实例需要能够自动摘除不健康实例，而且无需持久化存储实例，那么这种实例就适用于类 Gossip 的协议。右边的持久化实例使用服务端探测的健康检查方式，因为客户端不会上报心跳.那么自然就不能去自动摘除下线的实例。




![image.png](https://gitee.com/ycfan/images/raw/master/img/20231213130232.png)

#面试 
#### Nacos的服务注册表结构是怎样的？

Nacos采用了数据的分级存储模型，最外层是Namespace，用来隔离环境。然后是Group，用来对服务分组。接下来就是服务（Service）了，一个服务包含多个实例，但是可能处于不同机房，因此Service下有多个集群（Cluster），Cluster下是不同的实例（Instance）。

对应到Java代码中，Nacos采用了一个多层的Map来表示。结构为Map<String, Map<String, Service>>，其中最外层Map的key就是namespaceId，值是一个Map。内层Map的key是group拼接serviceName，值是Service对象。Service对象内部又是一个Map，key是集群名称，值是Cluster对象。而Cluster对象内部维护了Instance的集合。

#### Nacos如何支撑阿里内部数十万服务注册压力？

Nacos内部接收到注册的请求时，不会立即写数据，而是将服务注册的任务放入一个阻塞队列就立即响应给客户端。然后利用线程池读取阻塞队列中的任务，异步来完成实例更新，从而提高并发写能力。

#### Nacos如何避免并发读写冲突问题？

Nacos在更新实例列表时，会采用CopyOnWrite技术，首先将旧的实例列表拷贝一份，然后更新拷贝的实例列表，再用更新后的实例列表来覆盖旧的实例列表。

这样在更新的过程中，就不会对读实例列表的请求产生影响，也不会出现脏读问题了。

#### Nacos与Eureka的区别有哪些？

Nacos与Eureka有相同点，也有不同之处，可以从以下几点来描述：

- 接口方式：Nacos与Eureka都对外暴露了Rest风格的API接口，用来实现服务注册、发现等功能
- 实例类型：Nacos的实例有永久和临时实例之分；而Eureka只支持临时实例
- 健康检测：Nacos对临时实例采用心跳模式检测，对永久实例采用主动请求来检测；Eureka只支持心跳模式
- 服务发现：Nacos支持定时拉取和订阅推送两种模式；Eureka只支持定时拉取模式

  
