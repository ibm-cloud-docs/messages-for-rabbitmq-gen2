---
copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: rabbitmq, rabbitmq connecting, connecting to rabbitmq

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting an external application
{: #external-app}


[Gen 2]{: tag-purple}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.messages-for-rabbitmq-gen2_full}}. Your deployment has connection strings specifically for drivers, clients, and applications. Connection strings are displayed in the *Endpoints* panel of your deployment's *Overview*, and can also be retrieved from the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [{{site.data.keyword.databases-for}} API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to create users specifically for your applications to connect with. For more information, see [Creating Users and Getting Connection Strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings).

## Connecting with a language's driver
{: #connecting-lang-driver}

The information a driver needs to make a connection to your deployment is in the `amqps` section of your connection strings. The table contains a breakdown for reference.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for RabbitMQ, it is `uri`. |
| `Scheme` | | Scheme for a URI - for RabbitMQ, it is `amqps`. |
| `Path` | | Path for a uri. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment. |
| `Certificate` | `Base64` | A base64 encoded version of the certificate. |
{: caption="RabbitMQ/uri connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many RabbitMQ drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,

```sh
amqps://$USERNAME:$PASSWORD@f08da56c-f975-4cad-98a5-633b8b5a8e79.974350db55ab4ec0983f023940bf637f.databases.appdomain.cloud:30402
```

Here are a few of the common RabbitMQ drivers:

* [AMQP 0-9-1 library and client for Node.JS](https://www.npmjs.com/package/amqplib)
* [RabbitMQ Java Client Library](http://www.rabbitmq.com/java-client.html)
* [Bunny RabbitMQ Ruby Client](http://rubybunny.info/)
* [Pika Python protocol](https://pika.readthedocs.io/en/stable/)

## Connecting with a `STOMP` client
{: #connecting-stomp-client}

The information a STOMP client needs to make a connection to your deployment is in the `stomp_ssl` section of your connection strings. The table below contains a breakdown for reference.

| Field Name | Index | Description
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for STOMP, it is `stomp` |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD` |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the client. |
| `Hosts` | `0...` | A hostname and the STOMP-enabled port to connect to, as well as the protocol name "stomp-ssl" |
| `Composed` | `0...` | A URI combining Authentication, Host, and TLS/SSL |
| `ssl` | | The TLS/SSL setting needed for a connection. Should always be `true`. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment |
| `Certificate` | `Base64` | A base64 encoded version of the certificate. |
{: caption="RabbitMQ/stomp_ssl connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## Connecting with an `MQTT` client
{: #connecting-mqtt-client}

The information that an MQTT client uses to connect can be found in the `mqtts` section of your connection strings. The table contains a reference.

The "mqtts" section contains the information that an MQTT client needs to connect to your deployment.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for `MQTTS`, it is `uri`. |
| `Scheme` | | Scheme for a URI - in this case it is `mqtts`. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD` |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Authentication, Host, and Port used to connect. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment |
| `Certificate` | `Base64` | A base64 encoded version of the certificate. |
{: caption="RabbitMQ/mqtts connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## TLS and service proprietary certificate support
{: #tls-cert-support}

All connections to {{site.data.keyword.messages-for-rabbitmq-gen2}} are TLS 1.2 enabled, so the driver or client you use to connect needs to be able to support encryption. Your deployment also comes with a service proprietary certificate so the driver can verify the server upon connection.

For more information, see [{{site.data.keyword.databases-for}} Certificates FAQ](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-faq-cert){: external}.

### Using the service proprietary certificate
{: #using-selfsigned-cert}

1. Copy the certificate information from the *Endpoints* panel or the Base64 field of the connection information.
2. If needed, decode the Base64 string into text.
3. Save the certificate to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.
5. (optional) If your driver or client supports it you can add the certificate its (or your system's) certificate store.

### CLI plug-in support for the service proprietary certificate
{: #cli-plugin-support-selfsigned-cert}

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver or client.
