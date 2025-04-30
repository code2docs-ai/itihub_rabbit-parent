# 基础信息

|      |      |
|------|------|
| 名称 | config |
| 编码语言 | .java |
| 代码路径 | rabbit-parent/es-job/src/main/java/com/itihub/esjob/config |
| 包名 | rabbit-parent.docs.es-job.src.main.java.com.itihub.esjob.config |
| 概述说明 | Spring配置类实现ElasticJob事件记录、Zookeeper注册及分布式任务调度功能。 |

# 说明

# ElasticJob 配置模块总结

## 概述
该模块是基于Spring框架和ElasticJob实现的分布式任务调度系统，主要提供以下核心功能：
1. 与Zookeeper集成的注册中心配置
2. 多种作业类型支持（SimpleJob/DataflowJob）
3. 作业事件持久化到关系型数据库
4. 可配置的分布式调度策略

模块通过四个配置类实现完整调度功能，支持参数化配置、分片处理、作业监控等企业级特性。

## 主要业务场景
1. **分布式定时任务调度**：
   - 通过SimpleJob实现常规定时任务
   - 通过DataflowJob实现流式数据处理任务
   - 支持cron表达式配置调度策略

2. **作业事件追踪**：
   - 将作业执行事件（启动/完成/失败等）持久化到关系型数据库
   - 提供作业执行历史记录查询能力

3. **分布式协调**：
   - 基于Zookeeper实现作业节点注册与发现
   - 支持分片任务在集群中的自动分配
   - 配置超时控制（连接超时/会话超时）和重试机制

4. **弹性调度配置**：
   - 支持动态配置分片总数和分片参数
   - 可开启/关闭作业容错机制
   - 支持流式处理模式配置


### 包内部结构视图

```mermaid
graph TD
    config --> elasticejob
    elasticejob --> ElasticJobEventConfig.java
    elasticejob --> ElasticJobRegistryCenterConfig.java
    elasticejob --> MySimpleJobConfig.java
    elasticejob --> DataflowJobConfig.java
```

该流程图展示了ElasticJob配置模块的层级结构，根节点为config目录，其下包含elasticejob子目录。elasticejob目录中存放了四个具体的作业配置类文件，包括事件配置、注册中心配置以及两种不同类型的作业配置实现。这种结构清晰地体现了配置文件的组织方式，便于管理和维护作业配置相关代码。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [elasticejob](elasticejob/_module.md) | package | Spring配置类实现ElasticJob事件记录、Zookeeper注册及分布式任务调度功能。 |


