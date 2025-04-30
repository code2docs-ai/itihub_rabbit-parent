# 基础信息

|      |      |
|------|------|
| 名称 | RetryMessageDataflowJob |
| 编码语言 | .java |
| 代码路径 | rabbit-parent/rabbit-core-producer/src/main/java/com/itihub/rabbit/producer/task/RetryMessageDataflowJob.java |
| 包名 | com.itihub.rabbit.producer.task |
| 依赖项 | ['com.dangdang.ddframe.job.api.ShardingContext', 'com.dangdang.ddframe.job.api.dataflow.DataflowJob', 'com.itihub.rabbit.producer.broker.RabbitBroker', 'com.itihub.rabbit.producer.constant.BrokerMessageStatus', 'com.itihub.rabbit.producer.entity.BrokerMessage', 'com.itihub.rabbit.producer.service.MessageStoreService', 'com.itihub.rabbit.task.annotaion.ElasticJobConfig', 'com.itihub.rabbit.task.annotaion.JobCoreConfiguration', 'com.itihub.rabbit.task.annotaion.LiteJobConfiguration', 'lombok.extern.slf4j.Slf4j', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.stereotype.Component', 'java.util.List'] |
| 概述说明 | 定时任务补偿投递消息，最多重试3次，失败记录日志。 |

# 说明

该代码定义了一个名为RetryMessageDataflowJob的弹性作业类，用于处理可靠性投递消息的补偿任务。作业每10秒执行一次，单分片运行。类中注入了MessageStoreService和RabbitBroker服务，用于数据存取和消息投递。fetchData方法获取状态为SENDING的超时消息列表，processData方法处理这些消息：若重试次数超过3次则标记为失败，否则更新重试计数并重新投递消息。整个过程通过日志记录关键操作状态。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| RetryMessageDataflowJob | class | 定时任务补偿投递消息，每10秒执行一次，最多重试3次。 |



## 类 RetryMessageDataflowJob

|      |      |
|------|------|
| 访问范围 | @Slf4j;@ElasticJobConfig(;        coreConfig = @JobCoreConfiguration(;            name = "com.itihub.rabbit.producer.task.RetryMessageDataflowJob",;            cron = "0/10 * * * * ?",;            description = "可靠性投递消息补偿任务",;            shardingTotalCount = 1;        ),;        liteJobConfig = @LiteJobConfiguration(overwrite = true);;);public |
| 类型 | class |
| 名称 | RetryMessageDataflowJob |
| 说明 | 定时任务补偿投递消息，每10秒执行一次，最多重试3次。 |


### UML类图

```mermaid
classDiagram
    class RetryMessageDataflowJob {
        -MessageStoreService messageStoreService
        -RabbitBroker rabbitBroker
        -static final int MAX_RETRY_COUNT
        +RetryMessageDataflowJob(MessageStoreService messageStoreService, RabbitBroker rabbitBroker)
        +List~BrokerMessage~ fetchData(ShardingContext shardingContext)
        +void processData(ShardingContext shardingContext, List~BrokerMessage~ dataList)
    }

    class MessageStoreService {
        <<Interface>>
        +List~BrokerMessage~ fetchTimeOutMessage4Retry(BrokerMessageStatus status)
        +void failure(String messageId)
        +void updatedTryCount(String messageId)
    }

    class RabbitBroker {
        <<Interface>>
        +void reliantSend(Message message)
    }

    class BrokerMessage {
        +String messageId
        +Message message
        +int tryCount
        +String getMessageId()
        +Message getMessage()
        +int getTryCount()
    }

    class ShardingContext {
        // 分片上下文信息
    }

    RetryMessageDataflowJob --> MessageStoreService : 依赖
    RetryMessageDataflowJob --> RabbitBroker : 依赖
    RetryMessageDataflowJob --> BrokerMessage : 处理
    RetryMessageDataflowJob --> ShardingContext : 使用
```

该代码实现了一个基于ElasticJob的可靠性消息重试补偿任务。RetryMessageDataflowJob类通过定时任务（每10秒执行）从MessageStoreService获取超时未确认的消息，并通过RabbitBroker进行重试投递。当消息重试次数超过最大限制（3次）时标记为失败，否则更新重试计数并继续投递。类图展示了核心组件及其依赖关系，包括数据存储服务、消息代理接口和消息实体类。


### 内部方法调用关系图

```mermaid
graph TD
    A["类RetryMessageDataflowJob"]
    B["属性: MessageStoreService messageStoreService"]
    C["属性: RabbitBroker rabbitBroker"]
    D["常量: MAX_RETRY_COUNT = 3"]
    E["构造方法: RetryMessageDataflowJob(MessageStoreService, RabbitBroker)"]
    F["方法: List<BrokerMessage> fetchData(ShardingContext)"]
    G["方法: void processData(ShardingContext, List<BrokerMessage>)"]
    H["调用: messageStoreService.fetchTimeOutMessage4Retry()"]
    I["调用: messageStoreService.failure()"]
    J["调用: messageStoreService.updatedTryCount()"]
    K["调用: rabbitBroker.reliantSend()"]
    L["日志: 抓取数据集合"]
    M["日志: 消息重试失败告警"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    F --> H
    F --> L
    G --> I
    G --> J
    G --> K
    G --> M
```

该流程图展示了RetryMessageDataflowJob类的核心结构和执行逻辑。该类是一个ElasticJob定时任务，主要功能是处理消息重投递的补偿机制。通过fetchData方法获取待重试消息列表，processData方法实现消息重试逻辑：当重试次数超限时标记为失败，否则更新重试计数并重新投递。整个流程涉及消息存储服务和RabbitMQ代理的交互，并包含关键日志点用于监控。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| messageStoreService | MessageStoreService | 私有消息存储服务实例。 |
| MAX_RETRY_COUNT = 3 | int | 私有常量，最大重试次数为3。 |
| rabbitBroker | RabbitBroker | 私有RabbitMQ代理实例 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| processData | void | 处理消息重试逻辑：超最大次数标记失败，否则更新计数并重发。 |
| fetchData | List<BrokerMessage> | 重写fetchData方法，获取超时消息列表并返回。 |




