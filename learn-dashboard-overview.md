---

copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: deployment, crn, task, gui, api endpoint, dashboard

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# The Dashboard overview
{: #dashboard-overview}


[Gen 2]{: tag-purple}

The _Overview_ page shows information about your {{site.data.keyword.messages-for-rabbitmq-gen2_full}} deployment. The overview includes essential identifying information.

## Overview
{: #overview}

### Type
{: #type}

The type of database that is offered by the service, and the database version that your service uses.

### ID
{: #id}

The ID is a [CRN (Cloud Resource Name)](/docs/account?topic=account-crn) which uniquely identifies the database deployment. The CRN is used to refer to the database in the API and can be used with the CLI. The _Overview_ pane shows details of your service.

### Recent tasks
{: #recent-tasks}

Every time that you make administrative changes to your service (such as scaling, or taking a manual backup), a task starts up. The _Recent Tasks_ panel shows the task name and progress bar for any running tasks, and a list of the most recently completed tasks. Depending on how busy your deployment is, successful tasks can be shown for 24 - 48 hours. Unsuccessful tasks can show for 7 - 8 days. Tasks can also be retrieved from the [Cloud Databases API](/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymenttasks) and [CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-tasks-list). A historical record of tasks from any time period is available through the [{{site.data.keyword.atracker_full}} integration](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-at_events).

### Endpoints
{: #endpoints}

The _Endpoints_ pane within the _Overview_ pane contains connection strings for your deployment. Each tab contains connection information that is tailored to the type of connection or the protocol that uses it. Basic information includes things like _hostname_ and _port_, as well as the TLS service proprietary certificate, TLS/SSL parameters, and the default database of your deployment.

Reference tables for the different connection types are available on the [Getting credentials and connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings) page.

Connection strings reflect whether your deployment uses public endpoints, private endpoints or both. You can configure which endpoints are available on your deployment. For more information, see the [Service endpoints integration](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-service-endpoints&interface=ui) page.

You can manage your {{site.data.keyword.messages-for-rabbitmq-gen2}} service through the {{site.data.keyword.databases-for}} API. This panel provides the essential information for using the API. For more information about the {{site.data.keyword.databases-for}} API, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api) page.

## Resources
{: #resources}

The resources tab contains information and configuration options on the size and resource usage of your deployment. You can:

- [Scale disk, memory, and CPU](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-resources-scaling)
- [Configure Autoscaling](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-autoscaling)

## Backups
{: #backups}

The _Backups_ tab is the UI for managing your deployments backups. All of the available backups are listed with their timestamps. Click a backup to grab its ID or to restore it into a new deployment. More information is on the [Managing backups](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-backups-for-rabbitmq) page.

## Observability
{: #observability}

The _Observability_ tab provides access to the {{site.data.keyword.monitoringfull}}, logging, and event tracking integrations available for your deployment.

- [{{site.data.keyword.atracker_full}}](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-at_events)
- [{{site.data.keyword.logs_full}}](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-logging)
- [{{site.data.keyword.monitoringfull}}](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-monitoring)

## Settings
{: #settings}

The _Settings_ tab contains the UI for many of the tunable settings for your deployment. You can 
- view encryption details. Encryption at rest is enabled for all {{site.data.keyword.messages-for-rabbitmq-gen2}} deployments. If you brought your own encryption key from [Key Protect](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-key-protect&interface=ui), the panel provides a link to your Key Protect instance and the _Encryption Key_ field has the name of the key.
- [Change the admin password](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-user-management&interface=ui#user-management-set-admin-password-ui)
- [Implement or modify an IP allowlist](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-allowlisting&interface=ui)

## Service credentials
{: #service-credentials}

You can generate a new set of credentials for cases where you want to manually [connect an app](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-ibmcloud-app) or [external consumer](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-external-app) to an IBM Cloud service. [Learn more](/docs/account?topic=account-service_credentials).

## Connections
{: #connections}

Shows connected resources. You can use the `Create connection` button to bind this service to another resource.

## View docs
{: #view-docs}

The _View docs_ link from the `Actions` drop list opens the main documentation page for {{site.data.keyword.messages-for-rabbitmq-gen2}} in a new tab.
