# 基础信息

|      |      |
|------|------|
| 名称 | config |
| 编码语言 | .java |
| 代码路径 | rabbit-parent/rabbit-core-producer/src/main/java/com/itihub/rabbit/producer/config |
| 包名 | rabbit-parent.docs.rabbit-core-producer.src.main.java.com.itihub.rabbit.producer.config |
| 概述说明 | Spring配置类集合：配置RabbitMQ生产者的数据源、MyBatis组件、Mapper扫描及消息表初始化。 |

# 说明

## 概述
该代码模块是一个基于Spring框架的RabbitMQ生产者数据访问层配置模块，主要负责RabbitMQ生产者相关的数据库连接、MyBatis集成以及消息表结构初始化工作。模块通过多个配置类实现了数据源的创建与配置、MyBatis组件的初始化、Mapper接口的自动扫描以及消息表结构的自动化部署。

## 主要业务场景
1. **数据源配置**：通过`RabbitProducerDataSourceConfiguration`类加载配置文件并初始化Druid连接池，创建主数据源实例。
2. **MyBatis集成**：
   - `RabbitProduceMyBatisConfiguration`配置MyBatis核心组件（SqlSessionFactory和SqlSessionTemplate），指定数据源和Mapper XML路径。
   - `RabbitProducerMybatisMapperScannerConfig`自动扫描指定包路径下的Mapper接口并注册为Spring Bean。
3. **数据库表初始化**：`BrokerMessageTableConfiguration`通过执行SQL脚本自动创建和初始化RabbitMQ生产者消息表结构。
4. **依赖管理**：各配置类通过`@AutoConfigureAfter`确保正确的加载顺序，形成完整的数据访问层配置链。


### 包内部结构视图

```mermaid
graph TD
    config --> database
    database --> RabbitProduceMyBatisConfiguration.java
    database --> RabbitProducerMybatisMapperScannerConfig.java
    database --> RabbitProducerDataSourceConfiguration.java
    database --> BrokerMessageTableConfiguration.java
```

该流程图展示了RabbitMQ生产者模块中配置文件的层级结构。顶层是config目录，其下包含database子目录，database目录中包含四个MyBatis相关的配置类文件，分别处理数据源配置、Mapper扫描配置、MyBatis主配置和消息表配置。整个结构清晰地反映了生产者模块中数据库配置的组件关系。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [database](database/_module.md) | package | Spring配置类集合：配置RabbitMQ生产者的数据源、MyBatis组件、Mapper扫描及消息表初始化。 |


