---

copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: troubleshooting Messages for RabbitMQ, failure to connect, intermittent connection timeout, unable to connect

subcollection: messages-for-rabbitmq-gen2

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can’t I connect to my {{site.data.keyword.messages-for-rabbitmq-gen2_full}} deployment?
{: #troubleshoot-connect}

[Gen 2]{: tag-purple}

{: troubleshoot}
{: support}

If you encounter errors when connecting to your {{site.data.keyword.messages-for-rabbitmq-gen2_full}} deployment, review these common causes and solutions.
{: shortdesc}

You receive an error message or fail to connect to a {{site.data.keyword.messages-for-rabbitmq-gen2}} deployment. When reviewing application logs, you might see errors that mention intermittent connection timeouts or unable to connect.
{: tsSymptoms}

Review the following information to troubleshoot and resolve common connectivity problems:
{: tsResolve}

* An unsecured connection is a common cause of connectivity errors.  All {{site.data.keyword.messages-for-rabbitmq-gen2}} connections use TLS/SSL encryption. {{site.data.keyword.messages-for-rabbitmq-gen2}} rejects unsecured connections. To avoid errors, make sure you configured a secure connection. For more information, see [Getting started](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-getting-started).
* If you are using a private endpoint, make sure that you specify connection strings that contain the [Private endpoint](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-service-endpoints&interface=ui#private-endpoints-credentials) and that you followed the steps in [Connecting through private endpoints](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-service-endpoints&interface=ui#private-endpoint-connections).
* If your application log captures a short connection interruption, that behavior is expected as a normal part of operations for this managed service. Design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}. 
* If retrying your connection hasn't worked, reestablish your connection. If a connection has closed, retrying will have no effect. 
* If you experience several minutes of connection interruption, check the Cloud Status for the service. For more information, see [Application-level high-availability](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-ha-dr).

