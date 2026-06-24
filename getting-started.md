---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: rabbitmq, rabbitmq getting started

subcollection: messages-for-rabbitmq-gen2

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

[Gen 2]{: tag-purple}


## Gen 2 platform highlights
{: #gen2-highlights}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 includes important platform changes:
- **Protocols**: AMQP and MQTT only (Streams and STOMP not available)
- **Endpoints**: Private endpoints only
- **Queue types**: Quorum Queues are default; Classic Queues available but not highly available

- **Hosting**: Isolated Compute only (Shared Compute not available)
- **Scaling**: Not available at GA, coming soon after launch

If you have already created your deployment and want to connect to your RabbitMQ, you can skip to [getting your connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings&interface=ui) and [connecting with the RabbitMQ Management plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin).
{: .tip}

## Before you begin
{: #before-you-begin}

- You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.


## Create a {{site.data.keyword.messages-for-rabbitmq}} service instance
{: #create-service-instance}

### Log in to {{site.data.keyword.cloud_notm}}
{: step}

Log in to the {{site.data.keyword.cloud}} console.

### Select {{site.data.keyword.messages-for-rabbitmq}}
{: step}

Click the [{{site.data.keyword.messages-for-rabbitmq}} service](https://cloud.ibm.com/databases/messages-for-rabbitmq/create){: external} in the **catalog**.

### Select region
{: step}

You can select any location from the list.

 - The list shows all the regions where the service is available.
 - Gen 1 and Gen 2 tags differentiate between the offering platforms.

### Select platform
{: step}

Based on the region/location selected, choose the platform.

### Service details
{: step}

 - Enter the service name of your choice for this instance.
 - Select the resource group. Create one for your account if not already available.
 - Optional: Add tags to categorize your instance.

### Select the hosting model 
{: step}

This will be selected as Isolated Compute by default for Gen2.

### Resource allocation
{: step}

 - Select between Flex of Fixed profile type. Flex offers compute at a lower price point. Fixed offers compute at a higher price as it runs on the newest available compute.
 - Select Disk. Minimum 10 GB disk is offered.

### Service configuration
{: step}

 - Select the Database version. Always try to host your instance on the preferred version.
 - Select Encryption of your choice.
 - Service Endpoint is default selected as Private.

### Create
{: step}

Click **Create** to create a {{site.data.keyword.messages-for-rabbitmq}} service.

 - Once the instance is created, you find it in the _Integration section_ under the Resources list tab.

### Link your application
{: step}

Link your application with the instance created using the message protocols AMQP, MQTT or STOMP. Connection details can be found in the _Overview_ page of the provisioned resource.

You cannot connect an application to the service until provisioning is complete.
{: .tip}
