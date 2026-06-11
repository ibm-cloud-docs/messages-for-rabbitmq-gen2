---
copyright:
  years: 2026,
lastupdated: "2026-06-11"

keywords: rabbitmq, rabbitmq users

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing users and permissions
{: #user-management}

[Gen 2]{: tag-purple}

{{site.data.keyword.messages-for-rabbitmq_full}} uses RabbitMQ's [built-in access control](https://www.rabbitmq.com/access-control.html#permissions){: external}.
{: shortdesc}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 instances do not include a default admin user. Instead, you create users with the `Manager` or `Writer` role using the {{site.data.keyword.cloud_notm}} service credential interface — via UI, CLI, or API. You can also create users directly in RabbitMQ through the Management UI.

Since {{site.data.keyword.messages-for-rabbitmq}} comes with the RabbitMQ Management plug-in enabled, user access is also controlled by [user tags](https://www.rabbitmq.com/management.html#permissions){: external}. These tags control what information is available to users through the management UI, `rabbitmqadmin`, and the RabbitMQ HTTP API.

## Manager users
{: #manager-users}

Users created with the `Manager` role function as admin-like users and have full administrative privileges on your RabbitMQ deployment. Manager users can:

- Provision new virtual hosts (vhosts)
- Manage all other users' permissions and access
- Access all settings and configuration in the _Admin_ tab in the RabbitMQ Management UI
- Configure, write, and read on all virtual hosts
- View all connections, channels, and node-related information

Manager users are automatically tagged with the "administrator" and "monitoring" tags in RabbitMQ, providing full access to the management plug-in.

For instructions on creating Manager users, see [Creating users with administrative privileges](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-admin-user).

## Writer users
{: #writer-users}

Users created with the `Writer` role have more limited privileges:

- Full permissions (configure, write, and read) on the default virtual host
- Tagged with the "monitoring" tag, allowing access to the management plug-in
- Can view connections, channels, and node-related information
- Have a limited view of the _Admin_ tab

Writer users cannot create new virtual hosts or manage other users' permissions.

## Creating users through Service Credentials
{: #service-cred-user}

You can create users through the _Service Credentials_ panel in the {{site.data.keyword.cloud_notm}} console. For detailed instructions, see [Creating users and getting connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings).

When creating a service credential, you can select either:
- **Manager** role - Creates a user with full administrative privileges
- **Writer** role - Creates a user with limited privileges on the default virtual host

Users created through Service Credentials are integrated with {{site.data.keyword.cloud_notm}} [IAM](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-iam).

## Creating users through the CLI
{: #cli-user}
{: cli}

You can create users through the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/cli?topic=cli-install-ibmcloud-cli) using the `ibmcloud resource service-key-create` command.

To create a Manager user:

```sh
ibmcloud resource service-key-create <credential-name> Manager --instance-name <instance-name>
```
{: pre}

To create a Writer user:

```sh
ibmcloud resource service-key-create <credential-name> Writer --instance-name <instance-name>
```
{: pre}

Users created through the CLI have the same permissions as users created through Service Credentials. They do not appear in _Service Credentials_ by default, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_) if you choose.

## Creating users through the API
{: #api-user}
{: api}

You can create users through the [{{site.data.keyword.cloud_notm}} Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-key){: external}.

To create a Manager user:

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_keys \
  -H 'Authorization: Bearer <IAM_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "<credential-name>",
    "source": "<instance-crn>",
    "role": "crn:v1:bluemix:public:iam::::serviceRole:Manager"
  }'
```
{: pre}

To create a Writer user:

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_keys \
  -H 'Authorization: Bearer <IAM_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "<credential-name>",
    "source": "<instance-crn>",
    "role": "crn:v1:bluemix:public:iam::::serviceRole:Writer"
  }'
```
{: pre}

Users created through the API have the same permissions as users created through Service Credentials. They do not appear in _Service Credentials_ by default, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_) if you choose.

## Creating users directly in RabbitMQ
{: #rabbitmq-user}

You can bypass Service Credentials and create users directly in RabbitMQ. The RabbitMQ Management plug-in UI has a tab for user creation and management available to Manager users on your deployment.

To access the user management interface:

1. Log in to the RabbitMQ Management UI with a Manager user
2. Navigate to the **Admin** tab
3. Select **Users** to create and manage users

Users created directly in RabbitMQ:
- Do not appear in _Service Credentials_ by default, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_)
- Are not integrated with IAM controls, even if added to _Service Credentials_
- Must be managed through the RabbitMQ Management UI or API

## The `ibm` user
{: #ibm-user}

Do not create a user with the name `ibm`, as this user is used internally by the service. Creating an `ibm` user can disrupt the availability of your deployment.
{: important}

## User permissions and virtual hosts
{: #user-permissions-vhosts}

RabbitMQ uses a permission model based on virtual hosts (vhosts). Each user can be granted specific permissions on specific vhosts:

- **Configure** - Create and delete queues, exchanges, and bindings
- **Write** - Publish messages
- **Read** - Consume messages

Manager users have full permissions on all vhosts. Writer users have full permissions on the default vhost (`/`) only.

To grant additional permissions to users, log in to the RabbitMQ Management UI with a Manager user and use the **Admin** tab to manage user permissions.

For more information about RabbitMQ's access control model, see the [RabbitMQ Access Control documentation](https://www.rabbitmq.com/access-control.html){: external}.
