# AWS Lambda注意事项

## Lambda函数中的全局变量

在lambda的go处理器中声明全局变量只在一段时间内有效，因为Lambda上下文会保持一段时间，Lambda上下文切换后，全局变量的变更会消失。



## 指标计算

### 并发执行数

**基于流的事件源**

对于处理 Kinesis 或 DynamoDB 流的 Lambda 函数，分区数即为并发单位。如果您的流有 100 个活动分区，则最多会有 100 个 Lambda 函数调用并发运行。这是因为 Lambda 按顺序处理每个分片的事件。

**并非基于流的事件源**

如果您创建的 Lambda 函数处理的不是来自基于流的事件源的事件 (例如，Lambda 可处理来自 Amazon S3 或 API 网关 等其他事件源的每个事件)，则每个发布的事件就是一个工作单元 (采用并行方式，最多可达您的账户限制)。因此，这些事件源发布的事件数（或请求数）影响并发度。您可以使用此公式来估算并发 Lambda 函数调用数：

```
events (or requests) per second * function duration
```

例如，考虑一个处理 Amazon S3 事件的 Lambda 函数。假定 Lambda 函数平均用时 3 秒，Amazon S3 每秒发布 10 个事件。因此，您的 Lambda 函数有 30 个并发执行。



### 请求速率

请求速率是指调用您的 Lambda 函数的速率。对于除了基于流的服务之外的所有服务，请求速率是事件源生成事件的速率。对于基于流的服务，AWS Lambda 按下面所示计算请求速率：

```
request rate = number of concurrent executions / function duration
```

例如，如果一个流上有 5 个活动分区（即，您有 5 个并行运行的 Lambda 函数），并且您的 Lambda 函数用时大概 2 秒，则请求速率为 2.5 个请求/秒。