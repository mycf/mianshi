在 Dubbo 微服务体系中，注册中心是其核心组件之一。Dubbo 通过注册中心实现了分布式环境中各服务之间的注册与发现，是各个分布式节点之间的纽带。其主要作用如下:
- 动态加入。一个服务提供者通过注册中心可以动态地把自己暴露给其他消费者，无须消费者逐个去更新配置文件。
- 动态发现。一个消费者可以动态地感知新的配置、路由规则和新的服务提供者，无须重启服务使之生效。
- 动态调整。注册中心支持参数的动态调整，新参数自动更新到所有相关服务节点。
- 统一配置。避免了本地配置导致每个服务的配置不一致问题


工作流程
- 服务提供者启动时，会向注册中心写入自己的元数据信息,同时会订阅配置元数据信息。
- 消费者启动时，也会向注册中心写入自己的元数据信息，并订阅服务提供者、路由和配置元数据信息。
- 服务治理中心(dubbo-admin)启动时，会同时订阅所有消费者、服务提供者、路由和配置元数据信息。
- 当有服务提供者离开或有新的服务提供者加入时，注册中心服务提供者目录会发生变化，变化信息会动态通知给消费者、服务治理中心。
- 当消费方发起服务调用时,会异步将调用、统计信息等上报给监控中心(dubbo-monitor-simple)。
