---
copyright:
  years: 2026, 2024
lastupdated: "2024-20-09"

keyowrds: rabbitmq, shovel

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# RabbitMQ Shovel
{: #rabbitmq-shovel}


[Gen 2]{: tag-purple}

Shovel is a plug-in for RabbitMQ that enables you to define replication relationships between brokers.

## Prerequisite
{: #rabbitmq-shovel-prereqs}

Before you configure Shovel, make sure that you create a user with `admin` permission on both formations. For more information, see [Managing Users and Permissions](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-user-management){: external}.

## Configuring RabbitMQ Shovel
{: #rabbitmq-shovel-config}

Create your Shovel on the destination formation. In RabbitMQ versions 3.12 and newer, it is mandatory to add `verify_none` in the source and destination URI, as detailed in the configuration procedure.

Follow these steps to configure RabbitMQ Shovel:

1. Log in to the [RabbitMQ Management UI](https://www.rabbitmq.com/management.html){: external} and select the **Admin** tab.
1. Select *Shovel Management* from the list.
1. Add a new shovel with the following information, then click *Add shovel*:
   - **Name**: test-shovel
   - **Source**: AMQP 0.9.1
     - *URI*: `amqps://username:password@<source_hostname>:<source_port>?verify=verify_none`
     - *Queue_name*: `source_queue`
   - **Destination**: AMQP 0.9.1
     - *URI*: `amqps://username:password@<target_hostname>:<target_port>?verify=verify_none`
     - *Queue_name*: `target_queue`
1. Review the details to verify that the shovel is created correctly.
1. To verify Shovel status, select *Shovel status*.
1. Now you can add a message to your source queue and it will redirect to the destination queue.

Replication relationships between brokers can be created within same region or cross-region.
{: .tip}
