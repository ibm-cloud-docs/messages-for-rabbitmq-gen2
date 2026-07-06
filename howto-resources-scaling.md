---

copyright:
  years: 2026
lastupdated: "2026-06-22"

keywords: rabbitmq, databases, manual scaling, disk I/O, memory, CPU, rabbitmq scaling

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Scaling disk, memory, and CPU
{: #resources-scaling}

[Gen 2]{: tag-purple}

**Important**: Scaling of disk or compute (CPU and RAM) is **not available at GA** for {{site.data.keyword.messages-for-rabbitmq}} Gen 2. Scaling capabilities will be made available soon after the initial launch.
{: important}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 uses the [Isolated Compute](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-isolated-compute) hosting model exclusively. When scaling becomes available, you will be able to adjust resources by selecting different profile sizes.

## Gen 2 profile sizes
{: #gen2-profile-sizes}

Gen 2 offers two types of profiles optimized for different workload requirements:

- **Fixed profiles** - Predefined vCPU and RAM configurations on the newest CPU generation, providing consistent and predictable performance for production workloads.
- **Flex profiles** - Predefined vCPU and RAM configurations across CPU generations, offering cost-optimized performance for development and testing environments.

All Gen 2 deployments use Isolated Compute with dedicated resources.

## Resource breakdown
{: #resources-breakdown}

{{site.data.keyword.messages-for-rabbitmq}} deployments have three data members in a cluster, and resources are allocated to all three members equally. For example, the minimum storage of a RabbitMQ deployment is 3072 MB, which equates to an initial size of 1024 MB per member. 

Billing is based on the _total_ amount of resources that are allocated to the service.
{: tip}

### Disk usage
{: #disk-usage}

Storage shows the amount of disk space that is allocated to your service. Each member gets an equal share of the allocated space. Your data is replicated across three data members in the RabbitMQ cluster, so the total amount of storage you use is approximately three times the size of your data set.

Disk allocation affects the performance of the disk, with larger disks having higher performance. . Reaching IOPS limits consistently causes throughput and message processing delays, which can be alleviated by scaling up disk space.

You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: tip}

### RAM
{: #ram}

RabbitMQ throttles publishing when it detects it is using 40% of available memory to keep memory usage from growing uncontrollably during spike of activity. If you find that you regularly reach the limit, you can allocate more memory to your deployment. Adding memory to the total allocation adds memory to the members equally.

### Queue rebalancing
{: #queue-rebalancing}

If you notice that one RabbitMQ node is occupying significantly more resources than another, it is likely that the queues are not evenly distributed between the nodes. This can happen for the following possible reasons:

- You are connected to only one of the VIPs and all the queues are created on a single node.
- There was a rolling restart, which moves the queues to the node that was restarted first.

Triggering even distribution of queues causes load until all queues are evenly distributed so this action should not be performed while the deployment is under pressure or outscaled.
{: note}

To evenly distribute the queues, you can use the [RabbitMQ management API](https://rawcdn.githack.com/rabbitmq/rabbitmq-server/v3.11.2/deps/rabbitmq_management/priv/www/api/index.html){: external} to run an https `POST` call `/api/rebalance/queues` against your deployment.

### vCPU
{: #dedicated-cores}

If you find that your database workloads need more CPU resources, you can scale the amount of CPU allocated to your service. With Isolated Compute, select the CPU x RAM configuration that matches your resource needs.

## Scaling considerations
{: #scaling-considerations}

- Scaling your deployment up might cause your RabbitMQ to restart. If your scaled deployment needs to be moved to a host with more capacity, then the databases are restarted as part of the move.

- Scaling down RAM or CPU does not trigger restarts.

- Disk cannot be scaled down.

- Scaling between different Isolated Compute sizes moves your deployment to new hosts. Your databases are restarted as part of that move. As your deployment is moved to a new host, this can take longer than smaller resource increases.

- Similarly, drastically increasing RAM or disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.atracker_full}}](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-at_events).

- **Scaling is not available at GA** but will be made available soon after launch.
- Autoscaling is not available in Gen 2. Monitor your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-monitoring).
- When scaling becomes available, you will be able to scale manually as needed.

## Review current resources and hosting model
{: #review-resources-ui}
{: ui}

In the **Resources** tab, you can view your current resource allocations and hosting model. Gen 2 uses Isolated Compute exclusively.

**Note**: Scaling functionality in the UI will be available soon after GA launch.

## Review current resources and hosting model
{: #review-resources-cli}
{: cli}

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command.

For example, with the following command you can view the resource groups for a deployment named "example-deployment".


```sh
ibmcloud cdb deployment-groups example-deployment
```
{: pre}

This command produces the following output:

```sh
Group   member
Count   3
|
+   Memory
|   Allocation                      3072mb
|   Allocation per member           1024mb
|   Minimum                         3072mb
|   Step Size                       384mb
|   Adjustable                      true
|   Cpu Enforcement Ratio Ceiling   49152mb
|   Cpu Enforcement Ratio           8192mb
|
+   CPU
|   Allocation              0
|   Allocation per member   0
|   Minimum                 9
|   Step Size               3
|   Adjustable              true
|
+   HostFlavor
|   ID            b3c.8x32.encrypted
|   Name          8x32
|   HostingSize   s
|
+   Disk
|   Allocation              3072mb
|   Allocation per member   1024mb
|   Minimum                 3072mb
|   Step Size               3072mb
|   Adjustable              true
```
{: pre}

The deployment has three members, with 3072 MB of RAM disk allocated in total. The "per member" allocation is 1024 MB of RAM and 1024 MB of disk. The minimum value is the lowest the total allocation can be set. The step size is the smallest amount by which the total allocation can be adjusted.

## Resources and scaling in the CLI
{: #resources-scaling-cli}
{: cli}

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set in MB. For example, to scale the memory of the "example-deployment" to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), you use the following command:

```sh
ibmcloud cdb deployment-groups-set example-deployment member --memory 12288
```
{: pre}

## Determine the hosting model of your database
{: #resources-hosting-determine-cli}
{: cli}

Use the following command to review the value of the `hostflavor` attribute. 

```sh
ibmcloud cdb groups <deployment_id> --json
```
{: pre}




## Review current resources and hosting model
{: #review-resources-api}
{: api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [`/deployments/{id}/groups`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl) endpoint. 



## Determine the hosting model of your database
{: #resources-hosting-determine-api}
{: api}

Use the following command to review the value of the `host_flavor` attribute. 

```sh
curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups
-H 'Authorization: Bearer <>' \
```
{: pre}



### The `host flavor` parameter
{: #host-flavor-parameter-api}
{: api}

The `host_flavor` parameter defines your compute sizing. Choose from Fixed profiles for consistent performance or Flex profiles for cost optimization.

#### Fixed profiles
{: #fixed-profiles-api}

Fixed profiles provide predefined vCPU and RAM configurations on the newest CPU generation for consistent, predictable performance.

| **Host flavor** | **host_flavor value** |
|:-------------------------:|:---------------------:|
| 4 CPU x 20 RAM            | `bx3d.4x20.encrypted`    |
| 8 CPU x 40 RAM            | `bx3d.8x40.encrypted`    |
| 8 CPU x 80 RAM            | `mx3d.8x80.encrypted`    |
| 32 CPU x 160 RAM          | `bx3d.32x160.encrypted`  |
| 48 CPU x 240 RAM          | `bx3d.48x240.encrypted`  |
{: caption="Fixed profile sizing parameters" caption-side="bottom"}
{: #fixed_profiles_api_table_scaling}

#### Flex profiles
{: #flex-profiles-api}

Flex profiles provide predefined vCPU and RAM configurations across CPU generations for cost-optimized performance.

The `host_flavor` parameter defines your compute sizing. 


## Review current resources and hosting model
{: #review-resources-terraform}
{: terraform}

Review resource allocations to your database by checking your terraform scripts for `cpu { allocation_count = }`, `memory {allocation_mb = }`, and `disk { allocation_mb = }`. 
