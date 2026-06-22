---

copyright:
  years: 2026
lastupdated: "2026-06-22"

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

## 25 June 2026
{: #messages-for-rabbitmq-gen2-25jun2026}
{: release-note}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 is now generally available (GA)
: {{site.data.keyword.messages-for-rabbitmq}} Gen 2 includes the following platform changes and features:

**Protocol support**
: - AMQP and MQTT protocols only are available on Gen 2
  - Streams and STOMP protocols are not available on Gen 2

**Endpoints**
: - Private endpoints only on Gen 2 VPC
  - Public endpoints are not available

**Queue types**
: - Quorum queues are the default queue type on Gen 2
  - Classic queues are available but do not provide high availability (HA). Using this queue type configuration will be outside the scope of the support by the {{site.data.keyword.messages-for-rabbitmq}} service.
  - RabbitMQ has deprecated HA Classic queues, thus messages in Classic queues are not durable

**Backup and restore**
: - Gen 2 backups include both configuration data and message data (on Classic/Gen 1, only configurations were backed up)
  - Scheduled backups and decoupled backup storage are available

**Profile sizes**
: - Fixed profiles (newest CPU generation, consistent performance) and Flex profiles (across CPU generations, cost-optimized)
  - All deployments use isolated compute with dedicated resources

**Version management**
: - Gen 2 lists only major version series (for example, v4)
  - Minor and patch versions are automatically managed by {{site.data.keyword.cloud}}
  - This enables customers to run workloads on a major version for longer periods
  - {{site.data.keyword.ibm}} ensures seamless minor and patch version upgrades

**Scaling**
: - Scaling of disk or compute is not available at GA
  - Scaling capabilities will be made available soon after launch
  - Version upgrades will be made available immediately after GA
