---

copyright:
  years: 2026
lastupdated: "2026-06-15"

keywords: rabbitmq, databases, memory alarms, disk alarms, monitoring, disk I/O, rabbitmq performance

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Performance
{: #performance}

[Gen 2]{: tag-purple}

{{site.data.keyword.messages-for-rabbitmq_full}} deployments can be manually [scaled to your usage](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-resources-scaling). When you are tuning the performance of your deployment, consider a few factors.

Scaling of disk or compute is not available at MVP for Gen 2, but will be made available soon after launch.
{: note}

## Monitoring your deployment
{: #monitoring-deployment}

{{site.data.keyword.messages-for-rabbitmq}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-monitoring) for basic monitoring of resource usage on your deployment. Observing trends in your usage can help you plan for scaling when it becomes available and help alleviate performance problems before your databases become unstable due to resource exhaustion.

## RabbitMQ memory usage
{: #rabbitmq-mem-usage}

[RabbitMQ provides a robust breakdown of memory usage](https://www.rabbitmq.com/memory-use.html#breakdown){: external}, which can provide information on how memory resources are allocated and being used in your deployment. Most notably, connections, queue mirrors, and accumulated messages all use memory. If your use-case calls for many open connections at a time, you may need to  increase memory. For more information, see [RabbitMQ memory breakdown](https://www.rabbitmq.com/docs/memory-use#breakdown-intro){: external}.

Occasionally, RabbitMQ can experience memory spikes. Specifically, with {{site.data.keyword.messages-for-rabbitmq}} deployments, updates and maintenance where we restart or delete a node causes memory usage to increase to resync the restarted or new node. If your RabbitMQ consistently uses a high percentage of its available memory, one of these spikes can run your deployment out of memory and cause it to crash. It is a good idea to scale your memory so that it can accommodate resyncing a node.

Mirror Classic Queue is deprecated from RabbitMQ v4.x. Customers are expected to use Quorum Queues or Streams for durability of messages.
{: important}

### RabbitMQ memory alarms
{: #rabbitmq-mem-alarms}

By default, when the RabbitMQ server uses above 40% of the available RAM, it raises a memory alarm and blocks incoming messages from publishers. The memory alarm is cleared if consumers use enough of the messages or the messages are moved to disk. Once the memory alarm is cleared normal service resumes, and the publishers are unblocked. Note that this does not prevent the RabbitMQ server from using more than 40% of the allocated memory, it is merely the point at which publishers are throttled. For more information, see [RabbitMQ documentation](https://www.rabbitmq.com/memory.html){: external}.

## RabbitMQ disk alarms
{: #rabbitmq-disk-alarms}

By default, when the RabbitMQ server detects that free disk space has dropped below a certain threshold, it raises a disk alarm. The threshold for {{site.data.keyword.messages-for-rabbitmq}} is 80% of your deployment's disk size. The alarm blocks incoming messages from publishers and prevents messages in memory from being written to disk. The alarm is cluster-wide so if disk space on one node gets too low, the alarm blocks on all nodes. To clear the alarm, either messages that have been written to disk need to be consumed and that space is reclaimed, or scale your deployment to a larger disk size.

For more information about disk alarms, see the [RabbitMQ documentation](https://www.rabbitmq.com/disk-alarms.html){: external}.

## Disk IOPS
{: #disk-iops}

The number of input/output operations per second (IOPS) is limited by the type of storage volume that is being used. Storage volumes for {{site.data.keyword.messages-for-rabbitmq}} deployments are provisioned on [Block Storage Endurance Volumes in the 5 IOPS per GB tier](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui). IOPS limits can affect RabbitMQ message throughput and storage operations. Reaching these limits can cause disk to fall behind on reclaiming space after messages are consumed, leading to disk alarms and publisher throttling until activity slows down. You can increase the number IOPS available to your deployment by increasing disk space.

## Quorum queues
{: #quorum-queues}


High availability can be achieved with [quorum queues](https://www.rabbitmq.com/quorum-queues.html) that implement a durable, replicated queue based on the [Raft consensus algorithm](https://raft.github.io){:external}.

Quorum queues require memory and disk space for the write-ahead log (WAL) that is used to maintain state for operations. They also require disk I/O because all data is persisted to disk.

RabbitMQ documentation provides more details about their impact on [resource usage](https://www.rabbitmq.com/quorum-queues.html#resource-use) and [performance](https://www.rabbitmq.com/quorum-queues.html#performance).

Quorum queues are the default queue type for high availability starting with version 4. Mirrored (HA) classic queues are no longer supported after version 3.13. For more information, see the [release notes](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-relnotes&interface=ui#messages-for-rabbitmq-gen2-18mar2025).

## RabbitMQ alarm monitoring
{: #alarm-monitoring}

When a disk or memory alarm is triggered, RabbitMQ emits a [`connection.blocked` notification](https://www.rabbitmq.com/connection-blocked.html){: external} to publishing connections. Many drivers support the protocol necessary to catch the notification so you can design your application to respond to RabbitMQ alarms.

You can also monitor alarms from the [RabbitMQ HTTP API](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin#rabbitmq-management-http-api). Use the `GET /api/nodes` endpoints, and look for `mem_alarm` and `disk_free_alarm` in the response.

For more checks related to memory alarms, you can gather information that is related to a single node's memory by using the `GET /api/nodes/{node}/memory` endpoint.

## Standard health checks
{: #health-checks}

The [RabbitMQ HTTP API](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin#rabbitmq-management-http-api) provides a couple of [health check](https://www.rabbitmq.com/monitoring.html#health-checks){: external} endpoints to verify the state of the RabbitMQ nodes in your deployment.

- [All Nodes](https://www.rabbitmq.com/monitoring.html#node-metrics){: external} - `GET /api/healthcheck/node`
- [Single Node](https://www.rabbitmq.com/monitoring.html#node-metrics){: external} - `GET /api/healthcheck/node/<node_name>`

Health checks consume system resources. For smaller, less busy deployments, the health check shouldn't take long to give you a response. Larger deployments, or deployments under load, can take some time to return results.
{: #tip}
