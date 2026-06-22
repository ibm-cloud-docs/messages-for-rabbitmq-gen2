---

copyright:
  years: 2026
lastupdated: "2026-06-22"

keywords: messages, backups, rabbitmq backups, messages for rabbitmq backups

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Backups with {{site.data.keyword.messages-for-rabbitmq}}
{: #backups-for-rabbitmq}

[Gen 2]{: tag-purple}

On {{site.data.keyword.messages-for-rabbitmq_full}} Gen 2, backups include **both configuration data and message data**. This is a significant improvement over Gen 1 (Classic), where only configuration data was backed up.

When you restore from a backup on Gen 2, both your RabbitMQ configuration (users, virtual hosts, queues, exchanges, bindings, policies, and runtime parameters) and your message data will be restored.

Gen 2 uses **independent backups**, which are backup instances that operate independently from your database service instance. Independent backups persist even after the source database instance is deleted, providing greater flexibility for long-term data retention, compliance requirements, and disaster recovery scenarios.

For more information on backing up and restoring your {{site.data.keyword.messages-for-rabbitmq}} service, see the [Managing backups](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-dashboard-backups&interface=ui) section of the {{site.data.keyword.cloud}} documentation.

Review the [management responsibilities and terms and conditions](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-responsibilities-cloud-databases) that you have when you use {{site.data.keyword.messages-for-rabbitmq}}.


## Concepts and suggestions
{: #concepts-suggestions}

The expected operation for a {{site.data.keyword.messages-for-rabbitmq}} deployment is to keep queues short: messages are written and read in a short cycle with focus on throughput. {{site.data.keyword.messages-for-rabbitmq}} deployments are not intended as a data store like other {{site.data.keyword.cloud}} offerings.

For message delivery with durability and consistent quality of service, you must use [quorum queues](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr#ha-features). Quorum queues are the default queue type on Gen 2.

The [Shovel plug-in](https://www.rabbitmq.com/docs/shovel){: external} can also help move messages across instances and can be useful for migration scenarios.

## Independent backups
{: #independent-backups}

Independent backups represent a fundamental shift in how {{site.data.keyword.messages-for-rabbitmq}} Gen 2 manages backup data. Unlike traditional backups that are tightly coupled to your database instance lifecycle, independent backups exist as separate, provisionable service instances with their own lifecycle.

### Key features
{: #independent-backup-features}

Independent backups provide the following capabilities:

Separate lifecycle management
:   Each independent backup is a fully managed service resource with its own service name, Cloud Resource Name (CRN), and lifecycle management through {{site.data.keyword.cloud_notm}} Resource Controller.

Persistence after instance deletion
:   Backups persist even after the source {{site.data.keyword.messages-for-rabbitmq}} instance is deleted, enabling long-term data retention and compliance requirements.

Automatic and on-demand creation
:   Backups are created automatically on a daily schedule and can also be created on-demand at any time using the {{site.data.keyword.cloud_notm}} Resource Controller.

Cross-region backup copies
:   You can create copies of independent backups in different {{site.data.keyword.cloud_notm}} regions for enhanced disaster recovery.

Enhanced visibility
:   Independent backups appear in multiple locations: Database Hub, Resource List, and your instance's Backups and Restore tab.

Manual deletion control
:   Unlike coupled backups, independent backups can be manually deleted at any time through the Resource Controller, giving you greater control over backup data and associated costs.

### Backup retention
{: #backup-retention}

Independent backups follow a 30-day default retention period:

- Automatic daily backups are created according to your backup schedule
- Backups persist for 30 days by default
- Backups can be manually deleted before expiration if no longer needed
- After 30 days, backups are automatically deleted

### Viewing independent backups
{: #viewing-independent-backups}

Independent backups can be viewed in multiple locations:

- **Database Hub**: Provides a centralized view of all backups across your account, with separate tabs for backups whose source instance exists and those whose source instance has been deleted
- **Resource List**: Independent backups appear as separate service instances, allowing you to view backup details, manage lifecycle, and track costs
- **Instance Backups and Restore tab**: Shows all backups (both coupled and independent) associated with your specific instance

### Billing for independent backups
{: #independent-backups-billing}

Independent backups are billed as separate service instances:

- **Free allocation**: You receive free backup storage equal to the total provisioned disk size of your {{site.data.keyword.messages-for-rabbitmq}} deployment
- **Overage charges**: Usage beyond the free allocation is charged additionally
- **Cross-region copies**: Each copy in a different region is billed separately based on its size
- **Billing visibility**: Backup costs appear as separate line items in your billing statement

For detailed pricing information, see [Pricing](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-pricing).

### Security and compliance
{: #backup-security}

Independent backups maintain the same security standards as your database instances:

- **Encryption at rest**: All backups are encrypted using either IBM-managed keys or your own keys via Key Protect or Hyper Protect Crypto Services
- **Encryption in transit**: Data is encrypted during backup creation and restore operations
- **Access control**: IAM policies control who can create, view, and restore backups
- **Audit logging**: All backup operations are logged to {{site.data.keyword.atracker_full_notm}}

For more information, see [high availability and disaster recovery](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr).
