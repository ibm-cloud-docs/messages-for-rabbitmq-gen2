---
copyright:
  years: 2026
lastupdated: "2026-06-22"

keywords: rabbitmq, databases, rabbitmq connection strings

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Creating users and getting connection strings
{: #connection-strings}

[Gen 2]{: tag-purple}

To connect to {{site.data.keyword.messages-for-rabbitmq_full}}, you need some users and some connection strings. Connection Strings for your deployment are displayed on the _Dashboard Overview_, in the _Endpoints_ panel.

<!--
![Endpoints panel](images/getting-started-endpoints-panel.png){: caption="Endpoints panel" caption-side="bottom"}


You can also grab connection strings from the [CLI](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-cdb-reference) and the [API](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-api).

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 deployments do not include a default admin user. You must [create a Manager or Writer user](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-user-management#manager-users) through Service Credentials to connect to your deployment.
{: .tip}

## Credentials and connection strings for more users
{: #cred-connection-strings-additional-users}

You can create users with Manager or Writer roles by using the _Service Credentials_ panel, the {{site.data.keyword.IBM_notm}} CLI, or through the {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} API.

All users on your deployment can use the connection strings. Gen 2 supports **private endpoints only**.

Not all users get the same privileges regarding administering RabbitMQ. To read more about which users get what privileges see the [Managing Users](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-user-management) page.

### From the _Service Credentials_ UI
{: #cred-connection-strings-additional-users-service-cred}
{: ui}

1. Navigate to the service dashboard for your service.
2. Click _Service Credentials_ to open the _Service Credentials_ panel.
3. Click **New Credential**.
4. Choose a descriptive name for your new credential.
5. Gen 2 uses private endpoints only. Connection strings will automatically use private endpoints.
6. Click **Add** to provision the new credentials. A username and password, and an associated RabbitMQ user is auto-generated.

The new credentials appear in the table, and the connection strings are available as JSON in a click-to-copy field under _View Credentials_.

### From the CLI
{: #cred-connection-strings-additional-users-cli}
{: cli}

If you manage your service through the {{site.data.keyword.cloud_notm}} CLI and the [cloud databases plug-in](/docs/cli?topic=cli-install-ibmcloud-cli), you can create a new user with `cdb user-create`. For example, to create a new user for an "example-deployment", use the following command.
```sh
ibmcloud cdb user-create example-deployment <newusername> <newpassword>
```

Once the task is finished, you can retrieve the new user's connection strings with the `ibmcloud cdb deployment-connections` command.
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> [--endpoint-type <endpoint type>]
```

Full connection information is returned by the `ibmcloud cdb deployment-connections` command with the `--all` flag. To retrieve all the connection information for a deployment named "example-deployment", use the following command.
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> --all [--endpoint-type <endpoint type>]
```

If you don't specify a user, the `deployment-connections` commands return information for the first available user. Gen 2 deployments use private endpoints only, so you must specify `--endpoint-type private`.

To use the `ibmcloud cdb` CLI commands, you must [install the {{site.data.keyword.databases-for}} plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-cdb-reference).
{: .tip}

### From the API
{: #cred-connection-strings-additional-users-api}
{: api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. To create and manage users, use the base URL with the `/users` endpoint.

```sh
curl -X POST 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"username":"jane_smith", "password":"newsupersecurepassword"}'
```

To retrieve user's connection strings, use the base URL with the `/users/{userid}/connections` endpoint. You must specify in the path which user to use. Gen 2 uses private endpoints only.

```sh
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/{userid}/connections/{endpoint_type}'
```

### Adding users to _Service Credentials_
{: #adding-users-service-cred}

Creating a new user from the CLI or API doesn't automatically populate that user's connection strings into _Service Credentials_. If you want to add them there, you can create a new credential with the existing user information.

Enter the username and password in the JSON field _Add Inline Configuration Parameters_, or specify a file where the JSON information is stored. For example, `{"existing_credentials":{"username":"Robert","password":"supersecure"}}`.

Generating credentials from an existing user does not check for or create that user.
{: tip}

## Connection string breakdown
{: #connection-strings-breakdown}

### The `amqps` section
{: #amqps-section}

The "amqps" section contains information that is suited for your applications that make connections to RabbitMQ.

| Field name | Index | Description |
| ----------|-----|----------- |
| `Type` | | Type of connection - for RabbitMQ, it is "uri". |
| `Scheme` | | Scheme for a URI - for RabbitMQ, it is "amqps". |
| `Path` | | Path for a uri. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment. |
| `Certificate` | `Base64`|A base64 encoded version of the certificate. |
{: caption="RabbitMQ/uri connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

For more information on using this information to connect, see the [Connecting an External Application](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-external-app) page.

### The `stomp_ssl` section
{: #stomp_ssl-section}

**STOMP protocol is not available on Gen 2.** Gen 2 supports AMQP and MQTT protocols only.

This section is retained for reference purposes only and applies to Gen 1 (Classic) deployments.

### The `mqtts` section
{: #mqtts-section}

The `mqtts` section contains the information that an MQTT client needs to connect to your deployment.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for `MQTTS` it is `uri`. |
| `Scheme` | | Scheme for a URI - in this case it is `mqtts`. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method` | How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Authentication, Host, and Port used to connect. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment. |
| `Certificate` | `Base64` | A base64 encoded version of the certificate. |
{: caption="RabbitMQ/mqtts connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

### The CLI and https sections
{: #cli-https-section}

The `CLI` section contains information that is suited for the management plug-in and command-line clients that make connections to RabbitMQ.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Bin` | | The recommended binary to create a connection; in this case it is `rabbitmqadmin`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command-line parameters. |
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | `0...` | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | `Base64` | A service proprietary certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`.  |
{: caption="rabbitmqadmin/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

The `https` section contains information that you can use to access the RabbitMQ management plug-in via web browser.

| Field name|Index|Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for RabbitMQ, it is `uri`. |
| `Scheme` | | Scheme for a URI - for RabbitMQ, it is `https`. |
| `Path` | | Path for a URI. |
| `Authentication` | `Username`|The username that you use to connect. |
| `Authentication` | `Password`|A password for the user - might be shown as `$PASSWORD`. |
| `Authentication` | `Method`|How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to. |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate for database deployment. |
| `Certificate` | `Base64` | A base64 encoded version of the certificate. |
{: caption="https/uri connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

For more information, see [Connecting with the RabbitMQ Management plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin).
