---

copyright:
  years: 2026
lastupdated: "2026-06-05"

keywords: messages, backups, rabbitmq backups, messages for rabbitmq backups

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Backups with {{site.data.keyword.messages-for-rabbitmq}}
{: #backups-for-rabbitmq}

[Gen 2]{: tag-purple}

On {{site.data.keyword.messages-for-rabbitmq_full}} Gen 2, backups include **both configuration data and message data**. This is a significant improvement over Gen 1 (Classic), where only configuration data was backed up.

When you restore from a backup on Gen 2, both your RabbitMQ configuration (users, virtual hosts, queues, exchanges, bindings, policies, and runtime parameters) and your message data will be restored.

For more information on backing up and restoring your {{site.data.keyword.messages-for-rabbitmq}} service, see the [Managing backups](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-dashboard-backups&interface=ui) section of the {{site.data.keyword.cloud}} documentation.

Review the [management responsibilities and terms and conditions](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-responsibilities-cloud-databases) that you have when you use {{site.data.keyword.messages-for-rabbitmq}}.


## Concepts and suggestions
{: #concepts-suggestions}

The expected operation for a {{site.data.keyword.messages-for-rabbitmq}} deployment is to keep queues short: messages are written and read in a short cycle with focus on throughput. {{site.data.keyword.messages-for-rabbitmq}} deployments are not intended as a data store like other {{site.data.keyword.cloud}} offerings.

For message delivery with durability and consistent quality of service, you must use [quorum queues](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr#ha-features). Quorum queues are the default queue type on Gen 2.

The [Shovel plug-in](https://www.rabbitmq.com/shovel.html) can also help move messages across instances and can be useful for migration scenarios.

## Gen 2 backup features
{: #gen2-backup-features}

Gen 2 provides enhanced backup capabilities:

- **Scheduled backups**: Automatic backups run on a regular schedule
- **Decoupled backup storage**: Backups are stored separately from your deployment for additional protection
- **Configuration and message data**: Both are included in backups and can be restored

For more information, see [high availability and disaster recovery](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr).
