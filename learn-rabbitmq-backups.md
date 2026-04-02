---

copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: messages, backups, rabbitmq backups, messages for rabbitmq backups

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Backups with {{site.data.keyword.messages-for-rabbitmq-gen2}} 
{: #backups-for-rabbitmq}


[Gen 2]{: tag-purple}

The {{site.data.keyword.messages-for-rabbitmq-gen2_full}} backups do not contain the actual messages. Your {{site.data.keyword.messages-for-rabbitmq-gen2}} deployment backups contain *only* configuration data.  

For more information on backing up your {{site.data.keyword.messages-for-rabbitmq-gen2}} service configurations, see the [Managing backups](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-dashboard-backups&interface=ui) section of the {{site.data.keyword.cloud}} documentation. 

Review the [management responsibilities and terms and conditions](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-responsibilities-cloud-databases) that you have when you use {{site.data.keyword.messages-for-rabbitmq-gen2}}.


## Concepts and suggestions 
{: #concepts-suggestions}

The expected operation for a {{site.data.keyword.messages-for-rabbitmq-gen2}} deployment is to keep queues short: messages are written and read in a short cycle with focus on throughput. {{site.data.keyword.messages-for-rabbitmq-gen2}} deployments are not intended as a data store like other {{site.data.keyword.cloud}} offerings. 

For message delivery with durability and consistent quality of service, you must use [quorum queues](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr#ha-features). 

The [Shovel plug-in](https://www.rabbitmq.com/shovel.html) can also help move messages across instances. While not a backup mechanism, this plug-in can aid in message retention. 

Remember, messages are not part of the {{site.data.keyword.messages-for-rabbitmq-gen2}} deployment backups regardless of the queue types or plug-ins implemented. 

For more information, see [high availability and disaster recovery](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr). 


