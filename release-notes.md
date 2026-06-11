---

copyright:
  years: 2026
lastupdated: "2026-06-11"

keywords: messages-for-rabbitmq-gen2 release notes

subcollection: messages-for-rabbitmq-gen2

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #rabbitmq-relnotes}

[Gen 2]{: tag-purple}

Use these release notes to learn about the latest updates to {{site.data.keyword.messages-for-rabbitmq_full}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 19 May 2026
{: #messages-for-rabbitmq-gen2-19may2026}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 Platform Updates
: {{site.data.keyword.messages-for-rabbitmq}} Gen 2 includes the following platform changes and features:

**Protocol Support**
: - AMQP and MQTT protocols only are available on Gen 2
  - Streams and STOMP protocols are not available on Gen 2

**Endpoints**
: - Private endpoints only on Gen 2 VPC
  - Public endpoints are not available

**Queue Types**
: - Quorum Queues are the default queue type on Gen 2
  - Classic Queues are available but do not provide high availability (HA)
  - RabbitMQ has deprecated HA Classic Queues; messages in Classic Queues are not durable

**Backup and Restore**
: - Gen 2 backups include both configuration data and message data
  - On Classic (Gen 1), only configurations were backed up
  - Scheduled backups and decoupled backup storage are available

**Profile Sizes**
: - Fixed profiles (newest CPU generation, consistent performance) and Flex profiles (across CPU generations, cost-optimized)
  - All deployments use Isolated Compute with dedicated resources

**Version Management**
: - Gen 2 lists only major version series (e.g., v4)
  - Minor and patch versions are automatically managed by IBM Cloud
  - This enables customers to run workloads on a major version for longer periods
  - IBM ensures seamless minor and patch version upgrades

**Scaling**
: - Scaling of disk or compute is not available at MVP
  - Scaling capabilities will be made available soon after launch
  - Version upgrades will be made available immediately after MVP

## 27 March 2026
{: #databases-for-elasticsearch-27mar2026}
{: release-note}

Deprecation of {{site.data.keyword.hscrypto}}
: {{site.data.keyword.cloud}} is transitioning its dedicated key management offering from {{site.data.keyword.hscrypto}} to {{site.data.keyword.keymanagementservicelong}} Dedicated (Single Tenant). As part of this transition, {{site.data.keyword.hscrypto}} will reach **End of Life (EOL) on March 20, 2027**. After this date, the service will no longer be supported, and any remaining instances will be terminated.
To ensure continued service availability and support, you must migrate all existing HPCS root keys to {{site.data.keyword.keymanagementservicelong_notm}} Dedicated (Single Tenant) before the EOL date. For more information on how to migrate your encryption keys, see [Migrating from {{site.data.keyword.hscrypto}} (HPCS) to {{site.data.keyword.keymanagementserviceshort}} Dedicated (KP-ST)](/docs/cloud-databases?topic=cloud-databases-hpcs#migrating_hpcs_to_kp).

## 12 February 2026
{: #messages-for-rabbitmq-gen2-12february2026}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Version 4.2 is Preferred
: There are important changes in this version to align with the [RabbitMQ v4.2 release series](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v4.2.0){: external}.

  - RabbitMQ v4.2 is available on {{site.data.keyword.messages-for-rabbitmq}} in [Preferred](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-versioning-policy&interface=ui#version-tags). Service changes applied on {{site.data.keyword.messages-for-rabbitmq}} version 4.1 will continue on v4.2 and future versions. For more information, see the [version 4 realease notes](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-relnotes#messages-for-rabbitmq-gen2-10sep2025).
  - If you are using RabbitMQ v3.13 or RabbitMQ v4.1, you can upgrade to v4.2 directly using [backup and restore](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading&interface=ui) or by using [shovel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-shovel&interface=ui).
  - Some highlights of v4.2 are as follows:
      - You are encouraged to switch to [Quorum Queues](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-migrating_classic_quorum&interface=ui) with this version upgrade, because Classic Queues do not guarantee high availability of the messages.
      - Khepri will be the default datastore for the newly created instances.
      - Quorum queues now have a default [redelivery limit](https://www.rabbitmq.com/docs/next/quorum-queues#poison-message-handling){: external} set to 20. Messages that are redelivered 20 times or more will be [dead-lettered](https://www.rabbitmq.com/docs/dlx) or dropped (removed).


{{site.data.keyword.messages-for-rabbitmq}} version 4.1 End of Life on August 12 2026
: Action is required before August 12 2026 for RabbitMQ v4.1 deployments. After August 12 2026, active v4.1 instances will automatically be force upgraded to v4.2.
  - Deployments of IBM Cloud Messages for RabbitMQ v4.1 can be upgraded to v4.2.
  - Deployments of IBM Cloud Messages for RabbitMQ v3.13 can be upgraded to v4.2, skipping v4.1.
  - For more information, see [Upgrading to a new major version](https://cloud.ibm.com/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading&interface=ui) and [Migrating from Classic Queues to Quorum Queues](https://cloud.ibm.com/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-migrating_classic_quorum&interface=ui).


Policy update: reverting to Standard Versioning
: {{site.data.keyword.messages-for-rabbitmq}} is reverting the versioning policy [announced in November 2025](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-relnotes&interface=ui#messages-for-rabbitmq-gen2-05november2025). Starting with version v4.x, the following policy applies:
   - A 6-month notification and upgrade window will be provided.
   - The latest vendor versions are offered; you should upgrade as soon as new versions are available on {{site.data.keyword.cloud_notm}}.
   - Deprecated versions will be automatically force upgraded after the EOL date.

     No SLA is provided for instances upgraded using this automatic method.
     {: note}



## 5 November 2025
{: #messages-for-rabbitmq-gen2-05november2025}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version policy changes starting version v4.x
: To align with the open-source project owners of RabbitMQ, {{site.data.keyword.messages-for-rabbitmq}} (an {{site.data.keyword.cloud_notm}} managed service) is updating its version lifecycle policies starting with RabbitMQ v4.x.
  1. Two months advance notification of version deprecation will be provided for a major version.
  2. {{site.data.keyword.messages-for-rabbitmq}} will support only one version, the latest, outside of the few weeks period (minimum 2 weeks) during which you should upgrade from the version being deprecated to the new version.
  3. Instances that are using a deprecated version will be ‘automatically force upgraded’ to the next major version after the EOL date. However, no SLA will be provided for this upgrade method.

**Please note:**
We do not recommend that you wait until the end-of-life date for the following reasons:
   - We provide **no SLAs** for this type of forced upgrade.
   - You may experience some data loss.
   - Your application may experience downtime.
   - Your application may stop working if it has any incompatibilities with the new database version.
   - You cannot control the timing of when this upgrade will happen for your deployment.
   - There is no rollback process for this forced upgrade.

For more information on upcoming version EOL, see the [{{site.data.keyword.databases-for}} version policy page](/docs/cloud-databases?topic=cloud-databases-versioning-policy).

**Note**: {{site.data.keyword.messages-for-rabbitmq}} backup includes only configuration. Data (messages) is not backed up.

**Customer impact**: Frequent major version upgrades are expected with a shorter 2-month timeline to upgrade. Automatic forced upgrade will occur on EOL date.

**Customer action needed**: You are expected to [perform major version upgrade](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading&interface=ui) using *backup & restore* or *Shovel* before EOL date to align with the updated policy and to be on a supported version of the service.


## 10 September 2025
{: #messages-for-rabbitmq-gen2-10sep2025}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version 4.1 is Preferred
:  There are important changes in this version in alignment with the [RabbitMQ v4 release series](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v4.0.1).

RabbitMQ v4.1 is available on {{site.data.keyword.messages-for-rabbitmq}} in [Preferred](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-versioning-policy&interface=ui#version-tags). Service changes applied on {{site.data.keyword.messages-for-rabbitmq}} version 4.0 Preview will continue on v4.1 and future versions. For more information, see the [version 4 realease note](#messages-for-rabbitmq-gen2-18mar2025).

Customers on RabbitMQ v3.13 can upgrade to v4.1 using [backup and restore](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading&interface=ui) or by [shovel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-shovel&interface=ui). Customers are encouraged to switch to [Quorum Queues](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-migrating_classic_quorum&interface=ui) with this version upgrade, as Classic Queues does not guarantee high-availability of the messages.

## 18 March 2025
{: #messages-for-rabbitmq-gen2-18mar2025}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version 4 is available in Preview
: Validate your application on {{site.data.keyword.messages-for-rabbitmq}} version 4. There are important changes in this version in alignment with the [RabbitMQ v4 release](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v4.0.1).

  1. Mirrored (HA) Classic Queues will be unsupported: Starting version 4, [Mirrored Classic Queues](https://www.rabbitmq.com/docs/3.13/ha) will not be supported. When migrating from v3.13 to v4, remove the Classic Queue HA policy. Details can be found at [Migrating from Classic Queues to Quorum Queues](https://cloud.ibm.com/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-migrating_classic_quorum), and [Upgrading from v3.13 to v4](https://cloud.ibm.com/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading&interface=ui#upgrading-ui-v313-to-v4).

  2. Quorum Queues will be the default queue type: Until v3.13, Classic Queues were the default queue type selected when {{site.data.keyword.messages-for-rabbitmq}} instance was provisioned. Starting with v4, Quorum Queues will be the default queue type at the instance creation.

  3. Non-HA Classic Queue: You can still choose to use Classic Queues, however, it must be noted that messages cannot be highly-available due to the absence of Classic Queue HA policies. Using this configuration will be outside the scope of the support by the {{site.data.keyword.messages-for-rabbitmq}} service.

  4. Feature compatibility: While Quorum Queues are a better queue type when compared to Mirrored Classic Queues, they are not 100% compatible feature-wise with Mirrored Classic Queues. For more information, see the [feature matrix table](https://www.rabbitmq.com/docs/3.13/quorum-queues#feature-matrix) and [Migrate your RabbitMQ Mirrored Classic Queues to Quorum Queues](https://www.rabbitmq.com/docs/3.13/migrate-mcq-to-qq).

## 10 March 2025
{: #messages-for-rabbitmq-gen2-10mar2025}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} {{site.data.keyword.satelliteshort}} plan is deprecated
:   {{site.data.keyword.messages-for-rabbitmq}} {{site.data.keyword.satelliteshort}} plan is now deprecated. As of March 10 2025, all documentation relating to {{site.data.keyword.messages-for-rabbitmq}} {{site.data.keyword.satelliteshort}} plan has been removed, as well as the ability to select {{site.data.keyword.messages-for-rabbitmq}} {{site.data.keyword.satelliteshort}} plan in the Cloud console.

## 15 November 2024
{: #messages-for-rabbitmq-gen2-15nov2024}
{: release-note}

{{site.data.keyword.databases-for}} logs and events are now available on {{site.data.keyword.logs_full}}
: {{site.data.keyword.databases-for}} has onboarded {{site.data.keyword.logs_full_notm}}, a scalable logging service that persists logs and provides users with capabilities for querying, tailing, and visualizing logs. Use {{site.data.keyword.logs_full_notm}} to review your database logs and events starting **November 15, 2024**. For more information, see [Set up logging and monitoring](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-getting-started-cdb-logging-monitoring) and [About IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-about-cl).

## 29 September 2024
{: #messages-for-rabbitmq-gen2-29sept2024}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version 3.12 End of Life on April 30, 2025
:  Action is required before April 30, 2025, for your RabbitMQ 3.12 deployments. After April 30, 2025, {{site.data.keyword.messages-for-rabbitmq}} instances on version 3.12 that are still active will have their access removed, in line with our database versioning policy. You are expected to be on version 3.13, the latest version of {{site.data.keyword.messages-for-rabbitmq}}. For more information, see [Upgrading to a new major version](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading).

## 16 September 2024
{: #databases-for-rabbitmq-16sept2024}
{: release-note}

Private endpoints as new default
:  To ensure best possible security for your databases, private endpoints are now the default in the {{site.data.keyword.cloud_notm}} console. CLI and Terraform now require the endpoint type to be provided as part of creating an instance.

## 12 August 2024
{: #databases-for-rabbitmq-12aug2024}
{: release-note}

RabbitMQ v3.13 is live
: RabbitMQ v3.13 is available on {{site.data.keyword.messages-for-rabbitmq}}. It includes several new features, bug fixes, and optimizations, such as the following:
  - [MQTT QoS 0 queue type](https://www.rabbitmq.com/docs/mqtt#qos0-queue-type){: external} is available to be used if specific criteria is met.
  - [Overload protect](https://www.rabbitmq.com/docs/mqtt#overload-protection){: external} against high [memory usage](https://www.rabbitmq.com/docs/memory-use){: external} due to MQTT QoS 0 messages.
  - Deprecated features are now listed via https API on RabbitMQ Management UI and a warning is logged upon their usage. Adopt new stable features, and stop using deprecated features.
  - Visibility of enabled feature flags via https API and RabbitMQ management UI for administrator users.

## 1 May 2024
{: #databases-for-rabbitmq-01may2024}
{: release-note}

New hosting models
:  Gen 2 uses Isolated Compute exclusively, a secure single-tenant offering for complex, highly performant enterprise workloads with dedicated resources. For more information, see [Isolated Compute](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-isolated-compute).

## 12 February 2024
{: #messages-for-rabbitmq-gen2-12feb2024}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Streams Available
:  RabbitMQ Streams is a persistent replicated data structure that functions similarly to queues, buffering messages from producers for consumption by consumers.
For more information, see [RabbitMQ Streams](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-streams){: external}.

## 19 January 2024
{: #messages-for-rabbitmq-gen2-19jan2024}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version 3.11 End of Life on July 24, 2024
:  Action is required before July 24, 2024, for your RabbitMQ 3.11 deployments. After July 24, 2024, {{site.data.keyword.messages-for-rabbitmq}} instances on version 3.11 that are still active will have their access removed, in line with our Database Versioning Policy. You are expected to be on version 3.12, the latest preferred version of {{site.data.keyword.messages-for-rabbitmq}}.
For more information, see [Upgrading to a new Major Version](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading){: external}.

## 20 December 2023
{: #messages-for-rabbitmq-gen2-20dec2023}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Version 3.12 [Preferred](/docs/cloud-databases?topic=cloud-databases-versioning-policy#version-tags){: external} Release
:  For more information about provisioning a {{site.data.keyword.messages-for-rabbitmq}} instance, see [Provisioning](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-provisioning){: external}.

## 27 November 2023
{: #messages-for-rabbitmq-gen2-27nov2023}
{: release-note}

Monitoring Integration documentation updated
:  Monitoring Integration documentation now lists metrics for all {{site.data.keyword.databases-for}} services. For more information, see [Monitoring Integration](/docs/cloud-databases?topic=cloud-databases-monitoring){: external}.

## 08 November 2023
{: #messages-for-rabbitmq-gen2-08nov2023}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Version 3.12 [Preview](/docs/cloud-databases?topic=cloud-databases-versioning-policy#version-tags){: external} Release
:  Version 3.12 is released as a Preview. Features in this release:
   - [Optimizations](https://blog.rabbitmq.com/posts/2023/05/rabbitmq-3.12-performance-improvements/){: external} for both quorum and classic queues: improved throughput, lower throughput variability, lower latency, lower memory footprint.
   -  More mature and efficient implementation of (nonmirrored) classic queues v2 (CQv2).
   - Classic queue lazy and non-lazy modes no longer apply: classic queues v2 always behave similarly to the lazy mode in earlier release series: moving data to disk aggressively and only keeping a subset of data in memory.
   - Significantly [reduced MQTT and Web MQTT memory footprint per connection](https://blog.rabbitmq.com/posts/2023/03/native-mqtt/){: external}.
   - [RabbitMQ Shovel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-shovel){: external} plug-in now requires to have `verify_none` configured.

For more information, see [RabbitMQ 3.12 Release Notes](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.12.0){: external}.

For more information about provisioning a {{site.data.keyword.messages-for-rabbitmq}} instance, see [Provisioning](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-provisioning){: external}.

## 06 November 2023
{: #messages-for-rabbitmq-gen2-06nov2023}
{: release-note}

RabbitMQ Shovel Documentation Added
:  Shovel is a plug-in for RabbitMQ that enables you to define replication relationships between brokers. For more information, see [RabbitMQ Shovel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-shovel){: external}.

RabbitMQ Streams is now available for {{site.data.keyword.messages-for-rabbitmq}}. RabbitMQ Streams is similar to queues, but differs in how messages are stored and consumed. Streams are effective for large fan-outs, high throughput scenarios, and large logs. For more information, see [RabbitMQ's Streams]{https://www.rabbitmq.com/streams.html}{: external}.

## 01 September 2023
{: #messages-for-rabbitmq-gen2-01sept2023}
{: release-note}

RabbitMQ Streams now available

:  RabbitMQ Streams is now available for {{site.data.keyword.messages-for-rabbitmq}}. RabbitMQ Streams is similar to queues, but differs in how messages are stored and consumed. Streams are effective for large fan-outs, high throughput scenarios, and large logs. For more information, see [RabbitMQ's Streams](https://www.rabbitmq.com/streams.html){: external}.

## 29 August 2023
{: #messages-for-rabbitmq-gen2-29aug2023}
{: release-note}

Definition Troubleshooting Documentation Added
:  [RabbitMQ definitions](https://www.rabbitmq.com/definitions.html){: external} are metadata that RabbitMQ stores about its cluster. This metadata includes information about users, vhosts, queues, exchanges, bindings, and runtime parameters. Definitions can be used to restore a cluster or migrate to a new cluster. An error while importing definitions can be due to invalid imported arguments being imported from {{site.data.keyword.messages-for-rabbitmq}} 3.9 to version 3.11. For more information, see [Why can't I import definitions from {{site.data.keyword.messages-for-rabbitmq}} version 3.9 to version 3.11?](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-troubleshoot-defs).

## 20 July 2023
{: #messages-for-rabbitmq-gen2-20jul2023}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} version 3.9 End of Life on September 21, 2023
:  On September 21, 2023 {{site.data.keyword.messages-for-rabbitmq}} version 3.9 reaches its end of life. On that date, all {{site.data.keyword.messages-for-rabbitmq}} instances on version 3.9 that are still active will have their access removed, in line with our [Database Versioning Policy](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-versioning-policy){: external}. You are expected to be on version 3.11, the latest preferred version of {{site.data.keyword.messages-for-rabbitmq}}. If needed, [upgrade](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading) your database instances.

## 23 May 2023
{: #messages-for-rabbitmq-gen2-23may2023}
{: release-note}

Setting up disk alerts for disk utilization tutorial
:  In this tutorial, you use the {{site.data.keyword.cloud_notm}} API and the [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started){: external} to set up an alert that emails you whenever the disk utilization of your database exceeds 90%. This specific example creates an alert on a {{site.data.keyword.databases-for-elasticsearch}} deployment, but it is applicable to all the databases in the IBM {{site.data.keyword.databases-for}} catalog. For more information, see [Setting up disk alerts for disk utilization](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-disk-util-alert-tutorial).

## 09 March 2023
{: #messages-for-rabbitmq-gen2-09mar2023}
{: release-note}

RabbitMQ Version 3.11 Release
:  {{site.data.keyword.messages-for-rabbitmq_full}} version 3.11 is now available. With this release, messages that are delivered by a quorum queue and negatively acknowledged with a requeue are added to the back of the queue until the queue has redelivery limit set. With a redelivery limit, requeueing uses the original position of the message, if possible. With this update, if you get stuck or requeue deliveries at a high rate, you do not indefinitely grow the quorum queue Raft log, potentially driving the node out of disk space. If the original behavior is important to keep, ensure your applications' quorum queues have a redelivery limit set. This is a **potentially breaking change**. For more information, see [Upgrading to a new Major Version](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-upgrading).

Unmirrored Classic Queues Handling Change
:  With the release of version 3.11, unmirrored classic queue handling is changed. If a node was to be shut down during maintenance, an unmirrored classic queue was automatically moved to another node. With version 3.11, this behavior is changed to match the documented RabbitMQ behavior. An unmirrored classic transient queue is deleted and an unmirrored classic durable queue is unavailable during that time. For more information, see RabbitMQ's [Classic Queue Mirroring](https://www.rabbitmq.com/ha.html#non-mirrored-queue-behavior-on-node-failure).{: external}

`idle_since` field now uses RFC 3339 format
:  The `idle_since` field now uses RFC 3339 format. Here is a sample value with the previous format: `2022-03-22 11:39:37`. Here is a sample value with the new format: `2022-03-22T11:39:37.908+01:00`.

## 19 October 2022
{: #messages-for-rabbitmq-gen2-19oct2022}
{: release-note}

Deploying and Connecting a Cloud Databases Instance Tutorial
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and Connecting a {{site.data.keyword.databases-for}} Instance](/docs/messages-for-rabbitmq-gen2?topic=cloud-databases-create-instance-tutorial).

## 11 October 2022
{: #messages-for-rabbitmq-gen2-11oct2022}
{: release-note}

Protecting {{site.data.keyword.messages-for-rabbitmq_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud_notm}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting Cloud Databases resources with context-based restrictions](/docs/messages-for-rabbitmq-gen2?topic=cloud-databases-cbr&interface=ui).

## 25 January 2022
{: #messages-for-rabbitmq-gen2-25jan2022}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq_full}} 3.8 End of Life in July 2022
:  On July 12, 2022, Messages for RabbitMQ version 3.8 reaches its end of life.

## 30 June 2021
{: #messages-for-rabbitmq-gen2-30jun2021}
{: release-note}

General Availability of {{site.data.keyword.messages-for-rabbitmq_full}} support for IBM Cloud Databases enabled by IBM Cloud Satellite.
:  A distributed cloud provides consistent security and services across environments, centralized workload visibility, reduced latency, easier compliance and higher application development velocity.

## 21 July 2020
{: #messages-for-rabbitmq-gen2-21jul2020}
{: release-note}

IBM Cloud Messages for RabbitMQ 3.8 is Now Preferred
:  RabbitMQ 3.8 Preferred status.

## 13 April 2020
{: #messages-for-rabbitmq-gen2-13apr2020}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq_full}} autoscaling
:  We are excited to announce that autoscaling of your deployments based on disk capacity and disk I/O utilization is now available for {{site.data.keyword.messages-for-rabbitmq_full}} via the UI, API, and CLI.

## 6 August 2019
{: #messages-for-rabbitmq-gen2-06aug2019}
{: release-note}

New Regions Available for IBM Cloud Database Services
:  {{site.data.keyword.messages-for-rabbitmq_full}} is now available to be deployed in Seoul; South Korea; and Chennai, India.

## 19 December 2018
{: #messages-for-rabbitmq-gen2-19dec2018}
{: release-note}

General Availability of {{site.data.keyword.messages-for-rabbitmq_full}}
:  {{site.data.keyword.messages-for-rabbitmq_full}} added to the [IBM Cloud Databases](https://www.ibm.com/cloud/databases) family.
