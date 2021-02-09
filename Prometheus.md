核心组件

### Prometheus Server

Prometheus Server 并不直接服务监控特定的目标，主要负责数据的收集，存储并且对外提供数据查询支持，因此为了监控，需要使用到 Exporter。Prometheus 周期性的从 Exporter 暴露的 HTTP 服务地址（通常是 /metrics）拉取监控样本数据





### Exporters

Exporter 将监控数据采集的端点通过 HTTP 服务的形式暴露给 Prometheus Server, Prometheus Server通过访问该 endpoint 端点即可获取到监控数据

`Node Exporter`可以采集到主机的运行指标如 CPU，内存，磁盘等信息。

通过配置 `prometheus.yml`配置文件可以让`Prometheus`从 exporter 暴露的服务中获取指标数据

```
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```



比如监控指标

+ node_boot_time		系统启动时间
+ node_cpu			        系统 CPU 使用量
+ nodedisk*                    磁盘 IO
+ nodefilesystem*         文件系统用量
+ node_load1                 系统负载
+ nodememeory*          内存使用量
+ nodenetwork*            网络带宽
+ node_time                   当前系统时间
+ go_*                              node exporter 中 go 相关指标
+ process_*                     node exporter 自身进程相关运行指标



每个监控指标会有如下格式的信息：

HELP 用于解释当前指标的含义，TYPE 说明指标数据类型，数据类型。

数据类型比如 

+ counter 计数器（只增不减的度量指标），用于累计值
+ gauge 仪表盘（反映当前状态的数据可能增加或减少）
+ Histogram 直方图类型，主要用于表示一段时间范围内对数据进行采样，并对其指定区间以及总数进行统计
  + 事件发生的总次数，basename_count
  + 所有事件产生值的大小的总和，basename_sum
  + 事件产生的值分布在bucket中的次数，basename_bucket{le=“上包含”}
+ Summary 摘要类型，主要用于表示一段时间内数据采样结果，它直接存储了quantile数据，而不是根据统计区间计算出来的。

```
# HELP node_cpu Seconds the cpus spent in each mode.
# TYPE node_cpu counter
node_cpu{cpu="cpu0",mode="idle"} 362812.7890625
# HELP node_load1 1m load average.
# TYPE node_load1 gauge
node_load1 3.0703125
```



### AlertManager

在Prometheus Server中支持基于PromQL创建告警规则，如果满足PromQL定义的规则，则会产生一条告警，而告警的后续处理流程则由AlertManager进行管理。在AlertManager中我们可以与邮件，Slack等等内置的通知方式进行集成，也可以通过Webhook自定义告警处理方式。AlertManager即Prometheus体系中的告警处理中心。



### PushGateway

由于Prometheus数据采集基于Pull模型进行设计，因此在网络环境的配置上必须要让Prometheus Server能够直接与Exporter进行通信。 当这种网络需求无法直接满足时，就可以利用PushGateway来进行中转。可以通过PushGateway将内部网络的监控数据主动Push到Gateway当中。而Prometheus Server则可以采用同样Pull的方式从PushGateway中获取到监控数据。