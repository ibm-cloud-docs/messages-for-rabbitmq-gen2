---
copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: rabbitmq, rabbitmq connecting, connecting to rabbitmq

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting an external application
{: #external-app}

[Gen 2]{: tag-purple}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.messages-for-rabbitmq_full}}. Your deployment has connection strings specifically for drivers, clients, and applications. Connection strings are displayed in the *Endpoints* panel of your deployment's *Overview*, and can also be retrieved from the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-cdb-reference), and the [{{site.data.keyword.databases-for}} API](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-api).

The connection strings can be used by any of the credentials you created on your deployment. You can create Manager users (with full administrative privileges) or Writer users (with limited privileges) for your applications to connect with. For more information, see [Creating Users and Getting Connection Strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings).

## Connecting with a language's driver
{: #connecting-lang-driver}

The information a driver needs to make a connection to your deployment is in the `amqps` section of your connection strings. The table contains a breakdown for reference.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for RabbitMQ, it is `uri`. |
| `Scheme` | | Scheme for a URI - for RabbitMQ, it is `amqps`. |
| `Path` | | Path for a uri. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path. |

{: caption="RabbitMQ/uri connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many RabbitMQ drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,

```sh
amqps://$RABBITMQUSER:$RABBITMQPASS@f08da56c-f975-4cad-98a5-633b8b5a8e79.974350db55ab4ec0983f023940bf637f.databases.appdomain.cloud:30402
```

Here are a few of the common RabbitMQ drivers:

* [AMQP 0-9-1 library and client for Node.JS](https://www.npmjs.com/package/amqplib)
* [RabbitMQ Java Client Library](https://www.rabbitmq.com/client-libraries/java-client){: external}
* [Bunny RabbitMQ Ruby Client](http://rubybunny.info/)
* [Pika Python protocol](https://pika.readthedocs.io/en/stable/)

## Connecting with an `MQTT` client
{: #connecting-mqtt-client}

The information that an MQTT client uses to connect can be found in the `mqtts` section of your connection strings. The table contains a reference.

The "mqtts" section contains the information that an MQTT client needs to connect to your deployment.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for `MQTTS`, it is `uri`. |
| `Scheme` | | Scheme for a URI - in this case it is `mqtts`. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD` |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Authentication, Host, and Port used to connect. |
{: caption="RabbitMQ/mqtts connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

  ## Connecting with a `STOMP` client
{: #connecting-STOMP-client}

The connection details required by a STOMP client are provided in the **stomp_ssl** section of your connection strings.
The following table describes each parameter for reference.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for `STOMP`, it is `stomp`. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Authentication, Host, and Port that you use to connect. |
{: caption="RabbitMQ/mqtts connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.
* 
