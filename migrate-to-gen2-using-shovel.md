---

copyright:
  years: 2026
lastupdated: "2026-11-05"

keywords: rabbitmq migration, gen1 to gen2, classic infrastructure, vpc, rabbitmq shovel, shovel plugin, message migration, queue migration, private endpoints, live migration

subcollection: messages-for-rabbitmq-gen2

content-type: tutorial

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.messages-for-rabbitmq}} from Gen 1 to Gen 2 by using the Shovel plugin
{: #migrating-rabbitmq-gen1-to-gen2-shovel}
{: toc-content-type="tutorial"}

[Gen 2]{: tag-purple}

Use this tutorial to migrate messages from a {{site.data.keyword.messages-for-rabbitmq}} Gen 1 (Classic) deployment to a Gen 2 (VPC) deployment by using the RabbitMQ Shovel plugin.
{: shortdesc}

The RabbitMQ Shovel plugin runs inside the RabbitMQ server and continuously consumes messages from a queue on a source deployment and republishes them to a queue on a destination deployment over AMQP. This approach enables you to run your Gen 1 and Gen 2 deployments in parallel and transfer in-flight messages without message loss or extended maintenance downtime.

RabbitMQ Shovel is configured and managed directly through the RabbitMQ Management API. Customers who choose this approach configure and operate the Shovel parameters for their application queues.
{: note}

## How Shovel migration works
{: #how-shovel-works}

Migrating messages with the Shovel plugin follows these primary phases:

1. **Topology preparation**: You create the target queues, exchanges, and bindings on your new Gen 2 deployment so that it matches your Gen 1 configuration.
2. **Shovel creation**: You configure dynamic shovels on the Gen 2 (destination) deployment using the RabbitMQ Management API. The shovels connect to your Gen 1 queues over private AMQPS endpoints and forward messages to the Gen 2 queues.
3. **Producer cutover**: You switch your producer applications to publish new messages directly to the Gen 2 deployment.
4. **Queue draining**: The Shovel consumes all remaining messages from the Gen 1 source queues and moves them to Gen 2.
5. **Consumer cutover**: You switch your consumer applications to process messages from Gen 2.
6. **Cleanup**: Once all queues are drained and applications are verified on Gen 2, you delete the shovel configurations and decommission the Gen 1 deployment.

## Key considerations before you begin
{: #key-considerations}

Review these requirements and considerations before you configure the Shovel plugin:

### AMQPS URI TLS verification
{: #tls-verification-requirement}

For RabbitMQ versions 3.12 and newer, `verify=verify_none` must be specified in query parameters for both the source and destination AMQPS URIs when configuring Shovel parameters.
{: important}

### Network connectivity and private endpoints
{: #network-connectivity}

Gen 2 deployments use private endpoints only. Ensure that your Gen 2 deployment can reach the Gen 1 private endpoint over the {{site.data.keyword.cloud_notm}} private network (for example, through Classic Access, Direct Link, or Transit Gateway routing). Confirm that both deployments have private endpoint routing enabled before creating the Shovel.

### Queue and configuration replication
{: #queue-replication}

The Shovel plugin migrates messages between specific queues. It does not automatically replicate schema or topology definitions such as exchanges, bindings, or user permissions. You must define matching exchanges, queues, and bindings on the Gen 2 deployment before starting message transfer.

## Prerequisites
{: #prereqs}

Before configuring the Shovel plugin, ensure that you have:

* A Gen 1 {{site.data.keyword.messages-for-rabbitmq}} deployment running with existing queues and messages.
* A provisioned Gen 2 {{site.data.keyword.messages-for-rabbitmq}} deployment in your target VPC and region. Migration to Gen 2 is available in the us-east (Washington, DC), br-sao (São Paulo), and us-south (Dallas) regions, with additional regions to follow. Ensure that the region you specify is a supported Gen 2 region, such as `us-east`.
* Destination queues created on the Gen 2 deployment matching the source queue names and configurations.
* Service credentials (username, password, host, and port) for both Gen 1 and Gen 2 deployments.
* Network access to the RabbitMQ Management API on the Gen 2 deployment over port `15671`.
* `curl` and `jq` CLI utilities installed in your environment.

## Step 1: Retrieve connection details
{: #step-retrieve-credentials}
{: step}

Obtain the connection strings and credentials for both your Gen 1 and Gen 2 deployments:

1. In the {{site.data.keyword.cloud_notm}} console, navigate to your Gen 1 {{site.data.keyword.messages-for-rabbitmq}} deployment.
2. Select **Service credentials** and note the private AMQPS connection URI, username, password, hostname, and port.
3. Repeat the step for your Gen 2 {{site.data.keyword.messages-for-rabbitmq}} deployment. Note the private Management API hostname and port (`15671`) as well as the private AMQPS URI.

## Step 2: Configure target topology on Gen 2
{: #step-configure-topology}
{: step}

Ensure that all required exchanges, queues, and bindings exist on your Gen 2 deployment before starting the shovel. You can create them using the RabbitMQ Management UI, Management API, or your deployment automation scripts.

## Step 3: Create the dynamic Shovel on Gen 2
{: #step-create-shovel}
{: step}

Create the dynamic Shovel parameter on the destination (Gen 2) deployment using the RabbitMQ Management API.

Run the following `curl` command against the Gen 2 Management API endpoint:

```bash
curl -k -u '<GEN2_USERNAME>:<GEN2_PASSWORD>' \
  -X PUT 'https://<GEN2_HOST>:15671/api/parameters/shovel/%2F/<SHOVEL_NAME>' \
  -H 'content-type: application/json' \
  -d '{
    "component": "shovel",
    "name": "<SHOVEL_NAME>",
    "value": {
      "src-protocol": "amqp091",
      "src-uri": "amqps://<GEN1_USERNAME>:<GEN1_PASSWORD>@<GEN1_HOST>:<GEN1_PORT>?verify=verify_none",
      "src-queue": "<SOURCE_QUEUE>",
      "dest-protocol": "amqp091",
      "dest-uri": "amqps://<GEN2_USERNAME>:<GEN2_PASSWORD>@<GEN2_HOST>:<GEN2_PORT>?verify=verify_none",
      "dest-queue": "<DESTINATION_QUEUE>",
      "add-forward-headers": false,
      "ack-mode": "on-publish",
      "delete-after": "never"
    }
  }'
```
{: .pre}

Replace the following placeholder values:

* `<GEN2_USERNAME>` and `<GEN2_PASSWORD>`: Credentials for the Gen 2 deployment.
* `<GEN2_HOST>`: Private hostname of the Gen 2 deployment.
* `<SHOVEL_NAME>`: A descriptive identifier for this shovel (for example, `migrate-orders-queue`).
* `<GEN1_USERNAME>` and `<GEN1_PASSWORD>`: Credentials for the Gen 1 deployment.
* `<GEN1_HOST>` and `<GEN1_PORT>`: Private hostname and AMQPS port for the Gen 1 deployment.
* `<SOURCE_QUEUE>`: Name of the queue on the Gen 1 deployment.
* `<DESTINATION_QUEUE>`: Name of the destination queue on the Gen 2 deployment.

Key Shovel configuration parameters:

* `ack-mode`: Set to `"on-publish"` so messages are acknowledged on the Gen 1 source only after they are confirmed published to the Gen 2 destination.
* `delete-after`: Set to `"never"` to keep the shovel running continuously during the migration window, or `"queue-length"` if you want the shovel to terminate automatically once the source queue is empty.

## Step 4: Verify the Shovel status
{: #step-verify-shovel}
{: step}

Verify that the Shovel is active and connected.

To list all configured shovels on the Gen 2 deployment:

```bash
curl -sk -u '<GEN2_USERNAME>:<GEN2_PASSWORD>' \
  'https://<GEN2_HOST>:15671/api/shovels' | jq
```
{: .pre}

To inspect the status of a specific shovel:

```bash
curl -sk -u '<GEN2_USERNAME>:<GEN2_PASSWORD>' \
  'https://<GEN2_HOST>:15671/api/shovels' | \
  jq '.[] | select(.name=="<SHOVEL_NAME>")'
```
{: .pre}

A successfully running shovel outputs a status similar to the following example:

```json
{
  "name": "<SHOVEL_NAME>",
  "vhost": "/",
  "type": "dynamic",
  "pending": 0,
  "remaining": "unlimited",
  "remaining_unacked": "unlimited",
  "forwarded": 0,
  "state": "running",
  "src_protocol": "amqp091",
  "dest_protocol": "amqp091",
  "src_queue": "<SOURCE_QUEUE>",
  "dest_queue": "<DESTINATION_QUEUE>",
  "blocked_status": "running"
}
```
{: codeblock}

The `state: "running"` field confirms that the shovel has successfully connected to both the Gen 1 source and Gen 2 destination queues.

## Step 5: Verify message transfer
{: #step-verify-message-transfer}
{: step}

Validate that messages are transferring correctly from Gen 1 to Gen 2:

1. Publish a test message to `<SOURCE_QUEUE>` on the Gen 1 deployment.
2. Check `<DESTINATION_QUEUE>` on the Gen 2 deployment to confirm the message was received.
3. Query the Shovel status again and verify that the `forwarded` count increments accordingly.

## Step 6: Cut over applications
{: #step-cutover-apps}
{: step}

Once the Shovel is operational, perform the application cutover:

1. **Update producer applications**: Update your producer applications with the Gen 2 private connection endpoints and credentials. Restart the producers to start publishing new messages directly to Gen 2.
2. **Drain remaining Gen 1 messages**: Allow the active Shovel to forward any remaining messages from Gen 1 queues into Gen 2. Monitor the Gen 1 queue depth until all source queues reach zero messages.
3. **Update consumer applications**: Update your consumer applications with the Gen 2 private connection endpoints and credentials. Restart consumers to process messages directly from Gen 2.

## Step 7: Delete the Shovel and clean up
{: #step-cleanup-shovel}
{: step}

After all source queues are empty and producers and consumers are operating on Gen 2, remove the Shovel configuration from the Gen 2 deployment.

Run the following `curl` command to delete the Shovel parameter:

```bash
curl -k -u '<GEN2_USERNAME>:<GEN2_PASSWORD>' \
  -X DELETE 'https://<GEN2_HOST>:15671/api/parameters/shovel/%2F/<SHOVEL_NAME>'
```
{: .pre}

Confirm deletion by querying the shovels endpoint:

```bash
curl -sk -u '<GEN2_USERNAME>:<GEN2_PASSWORD>' \
  'https://<GEN2_HOST>:15671/api/shovels' | jq
```
{: .pre}

## Post-migration tasks
{: #post-migration}

After completing the migration:

* Confirm that all producers and consumers are actively sending and receiving messages on the Gen 2 deployment.
* Monitor queue depths, consumer connection counts, and resource utilization in IBM Cloud Monitoring.
* Retain the Gen 1 deployment through your organization's required rollback and verification window.
* Decommission and delete the Gen 1 deployment once validation is complete.
