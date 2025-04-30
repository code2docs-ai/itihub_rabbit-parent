# 基础信息

|      |      |
|------|------|
| 名称 | handler |
| 编码语言 | .java |
| 代码路径 | rabbit-parent/rabbit-common/src/main/java/com/itihub/rabbit/common/mybatis/handler |
| 包名 | rabbit-parent.docs.rabbit-common.src.main.java.com.itihub.rabbit.common.mybatis.handler |
| 概述说明 | MessageJsonTypeHandler处理Message对象与JSON的数据库读写转换。 |

# 说明

MessageJsonTypeHandler是一个继承自BaseTypeHandler的自定义类型处理器，用于处理Message类与数据库之间的JSON格式转换。它实现了四个核心方法：setNonNullParameter将Message对象转换为JSON字符串并存入PreparedStatement；三个getNullableResult方法分别从ResultSet的列名/列索引和CallableStatement中获取JSON字符串，若非空则转换为Message对象，否则返回null。所有方法都通过FastJsonConvertUtil工具类实现JSON与对象的互转。


### 包内部结构视图

```mermaid
graph TD
    handler --> MessageJsonTypeHandler.java
```

该流程图展示了rabbit-common项目中mybatis处理器模块的层级结构。handler目录作为父节点，包含一个具体的类型处理器实现文件MessageJsonTypeHandler.java。这种结构体现了MyBatis类型处理器的标准组织方式，其中基础目录存放特定功能的处理器实现类，用于处理JSON格式的消息数据与Java对象之间的转换。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [MessageJsonTypeHandler.java](MessageJsonTypeHandler.md) | file | MessageJsonTypeHandler处理Message对象与JSON的数据库读写转换。 |


