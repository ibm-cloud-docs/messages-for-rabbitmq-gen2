---
copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: rabbitmq, rabbitmq users

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing users and permissions
{: #user-management}


[Gen 2]{: tag-purple}

{{site.data.keyword.messages-for-rabbitmq-gen2_full}} uses RabbitMQ's [built-in access control](https://www.rabbitmq.com/access-control.html#permissions). 

When you provision a new deployment in {{site.data.keyword.cloud_notm}}, you are automatically given an `admin` user to access and manage RabbitMQ. You can also add users in the _Service Credentials_ panel, which allows for access to RabbitMQ to be integrated with your {{site.data.keyword.cloud_notm}} account and [IAM](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-iam with the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin), or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction).

Since {{site.data.keyword.messages-for-rabbitmq-gen2}} comes with the RabbitMQ Management plug-in enabled, user access is also controlled by [user tags](https://www.rabbitmq.com/management.html#permissions){: external}. These tags control what information is available to users through the management UI, `rabbitmqadmin`, and the RabbitMQ HTTP API.

## The `admin` user
{: #admin-user}

Every RabbitMQ deployment comes with an `admin` user. This `admin` user had full administrative privileges on your RabbitMQ deployment. The primary difference between the admin user and any other users you add to your deployment is the ability to provision new vhosts and manage all other users' permissions and access. `admin` is the only user that is initially granted access to all the settings and configuration that is found in the _Admin_ tab in the management UI. 

Before you log in with the admin user, set the password.

### Setting the Admin password in the UI
{: #user-management-set-admin-password-ui}
{: ui}

Set your Admin password through the UI by selecting your instance from the Resource List in the [{{site.data.keyword.cloud_notm}} Dashboard](https://cloud.ibm.com/){: external}. Then, select **Settings**. Next, select *Change Database Admin Password*.

### Setting the Admin password in the CLI
{: #user-management-set-admin-password-cli}
{: cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to set the admin password.

For example, to set the admin password for a deployment named `example-deployment`, use the following command:

```sh
ibmcloud cdb user-password example-deployment admin <newpassword>
```
{: pre}

### Setting the Admin password in the API
{: #user-management-set-admin-password-api}
{: api}

The Foundation Endpoint that is shown on the Overview panel Deployment Details section of your service provides the base URL to access this deployment through the API. Use it with the [Set specified user's password](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#changeuserpassword){: external} endpoint to set the admin password.

```sh
curl -X PATCH `https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/users/admin` \
-H `Authorization: Bearer <>` \
-H `Content-Type: application/json` \ 
-d `{"password":"newrootpasswordsupersecure21"}` \
```
{: pre}

## _Service credential_ users
{: #service-cred-user}

Users that you [create through the _Service Credentials_ panel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings) are given full permissions to configure, write, and read on the default Virtual Host.  

They are also automatically tagged with the "monitoring" tag, allowing users to access the management plug-in and see all connections, channels, and node-related information. These users given a limited view of the _Admin_ tab and the functions that are found there. 

If you need users that are created from _Service Credentials_ to have more privileges, you can log in with the admin user and grant them.

## Users created through the CLI
{: #cli-user}
{: cli}

Users that you create through the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/cli?topic=cli-install-ibmcloud-cli) are given the same permissions as _Service credential_ users. They have full permissions on the default Virtual Host and are tagged with the "monitoring" tag. If you need them to have more privileges, you can grant them while logged in with the admin user.

Users that are created directly from the CLI do not appear in _Service credentials_, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_) if you choose.

## Users created through the API
{: #api-user}
{: api}

Users that you create through the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction) are given the same permissions as _Service Credential_ users. They have full permissions on the default Virtual Host and are tagged with the "monitoring" tag. If you need them to have more privileges, you can grant them while logged in with the admin user.

Users that are created directly from the API do not appear in _Service Credentials_, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_) if you choose.

## RabbitMQ users
{: #rabbitmq-user}

Bypass creating users in _Service Credentials_ and create users directly in RabbitMQ. The RabbitMQ Management plug-in UI has a tab for user creation and management available to the admin user on your deployment.

Users who are created directly in RabbitMQ do not appear in _Service Credentials_, but you can [add them](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#adding-users-to-_service-credentials_). These users will not be integrated with IAM controls, even if added to _Service Credentials_.

## The `ibm` user
{: #ibm-user}

If you log in to the management UI with your `admin` user, don't create a user with name `ìbm`, as this user is used internally. Creating an ibm account is not advised and can disrupt the availability of your deployment.
