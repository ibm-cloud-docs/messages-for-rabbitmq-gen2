---
copyright:
  years: 2026
lastupdated: "2026-06-05"

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

This tutorial uses a [sample app](https://github.com/IBM-Cloud/clouddatabases-helloworld-cloudfoundry-examples/tree/node/rabbitmq){: .external} to demonstrate how to connect a Cloud Foundry application in {{site.data.keyword.cloud_notm}} to an {{site.data.keyword.messages-for-rabbitmq_full}} service. The application creates, reads from, and writes to a database that uses data that is supplied through the app's web interface.
{: shortdesc}

## Gen 2 platform highlights
{: #gen2-highlights}

{{site.data.keyword.messages-for-rabbitmq}} Gen 2 includes important platform changes:
- **Protocols**: AMQP and MQTT only (Streams and STOMP not available)
- **Endpoints**: Private endpoints only
- **Queue types**: Quorum Queues are default; Classic Queues available but not HA
- **Backups**: Include both configuration and message data
- **Hosting**: Isolated Compute only (Shared Compute not available)
- **Scaling**: Not available at MVP, coming soon after launch

If you have already created your deployment and want to connect to your RabbitMQ, you can skip to [getting your connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings&interface=ui) and [connecting with the RabbitMQ Management plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin).
{: .tip}

## Before you begin
{: #before-you-begin}

- You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: .external}.
- Install [Node.js](https://nodejs.org/){: .external} and [Git](https://git-scm.com/downloads){: .external}.
- See [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) for guidance on setting up a basic {{site.data.keyword.messages-for-rabbitmq_full}} deployment.

## Create a {{site.data.keyword.messages-for-rabbitmq}} service instance
{: #create-service-instance}
{: step}

Create a {{site.data.keyword.messages-for-rabbitmq}} service from the [{{site.data.keyword.messages-for-rabbitmq}} page](https://cloud.ibm.com/databases/messages-for-rabbitmq-gen2/create) in the {{site.data.keyword.cloud_notm}} catalog.

Choose a service name, region, organization, and space to provision the service in, and for the **Select a database version** field, choose _Latest Preferred Version_. In this example, the service name is "example-rabbitmq".

Click **Create** to provision your service. Provisioning can take a while to complete. You are taken back to your {{site.data.keyword.cloud_notm}} _Dashboard_ while the service is provisioning.

You cannot connect an application to the service until provisioning is complete.
{: .tip}

## Clone the Hello World sample app from GitHub
{: #clone-sample-app}
{: step}

Use the following command to clone the Hello World app to your local environment from your terminal:

```sh
git clone -b node git@github.com:IBM-Cloud/clouddatabases-helloworld-cloudfoundry-examples.git
```
{: .pre}

## Install the app dependencies
{: #install-app-dependencies}
{: step}

Use npm to install dependencies.

From your terminal, change the directory to where the sample app is located.

Install the dependencies listed in the `package.json` file.

```sh
npm install
```
{: .pre}

## Download and install the {{site.data.keyword.cloud_notm}} CLI tool
{: #install-cli-tool}
{: step}

The {{site.data.keyword.cloud_notm}} CLI tool is what you use to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line. For more information, see [Download and install {{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli).

## Connect to {{site.data.keyword.cloud_notm}}
{: #connect-cloud}
{: step}

Connect to {{site.data.keyword.cloud_notm}} in the command-line tool and follow the prompts to log in.

```sh
ibmcloud login
```
{: .pre}

If you have a federated user ID, use the `ibmcloud login --sso` command to log in with your single sign-on ID. See [Logging in with a federated ID](/docs/cli/login_federated_id.html#federated_id) to learn more.{: .tip}

Make sure that you are targeting the correct {{site.data.keyword.cloud_notm}} org and space.

```sh
ibmcloud target --cf
```
{: .pre}

By using the same values that you used when creating the service, choose from the provided options.

## Create a Cloud Foundry alias for the database service
{: #create-cloudfoundry-alias}
{: step}

{{site.data.keyword.ibmcf_full}} is deprecated. As of 30 November 2022 new {{site.data.keyword.ibmcf_full}} applications cannot be created and only existing users will be able to deploy applications. End-of-support happens on 1 June 2023. Any instances that still exist on 1 June 2023 will be deleted. For more information, see [Deprecation of IBM Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-deprecation).
{: deprecated}

Make the database service discoverable by {{site.data.keyword.ibmcf_full}} applications by giving it a {{site.data.keyword.ibmcf_full}} alias.

`ibmcloud resource service-alias-create alias-name --instance-name instance-name`

The alias name can be the same as the database service instance name. For example, use this command for database that was created in step 1.

`ibmcloud resource service-alias-create example-rabbitmq --instance-name example-rabbitmq`

## Update the app's manifest file
{: #update-app-manifest}
{: step}

{{site.data.keyword.cloud_notm}} uses a manifest file - `manifest.yml` to associate an application with a service. Follow these steps to create your manifest file.

In an editor, open a new file and add the following text:

```sh
---
applications:
- name:    example-helloworld-nodejs
  routes:
  - route: example-helloworld-nodejs.us-south.cf.appdomain.cloud
  memory:  128M
  services:
    - example-rabbitmq
```
{: .pre}

Change the `route` value to something unique. The route that you choose determines the subdomain of your application's URL:  `<route>.{region}.cf.appdomain.cloud`. Be sure the `{region}` matches where your application is deployed.

Change the `name` value. The name that you choose is displayed in your {{site.data.keyword.cloud_notm}} dashboard.

Update the `services` value to match the alias of the service you created in [Create a Cloud Foundry alias for the database service](#create-alias).

## Push the app to {{site.data.keyword.cloud_notm}}.
{: #push-app}
{: step}

If the service is not finished provisioning from Step 1, this step fails. You can check its progress on your {{site.data.keyword.cloud_notm}} _Dashboard_.
{: .tip}

When you push the app, it is automatically bound to the service specified in the manifest file.

```sh
ibmcloud cf push
```
{: .pre}

## Check that the app is connected to your {{site.data.keyword.messages-for-rabbitmq}} service
{: #check-app-connection}
{: step}

Go to your {{site.data.keyword.messages-for-rabbitmq}} service dashboard

Select _Connections_ from the dashboard menu. Your application is listed under _Connected Applications_.

If your application is not listed, repeat Steps 7 and 8, making sure that you entered the correct details in [manifest.yml](#step-8-push-the-app-to-sitedatakeywordcloudnotm).

## Use the app
{: #use-app}
{: step}

Now, when you visit `<route>.{region}.cf.appdomain.cloud/` you can see the contents of your {{site.data.keyword.messages-for-rabbitmq}} collection. As you add words and their definitions, they are added to the database and displayed. If you stop and restart the app, you see any words and definitions that were already added are now listed.

## Running the app locally
{: #run-app-local}

Instead of pushing the app into {{site.data.keyword.cloud_notm}} you can run it locally to test the connection to your {{site.data.keyword.messages-for-rabbitmq}} service instance. To connect to the service, you need to create a set of service credentials.

- Select _Service credentials_ from the main menu to open the Service Credentials view.
- Click **New credential**.
- Choose a name for your credentials and click **Add**.
- Your new credentials are now listed. Click **View credentials** in the corresponding row of the table to view the credentials, and click the **Copy** icon to copy your credentials.
- In your editor of choice, create a new file with the following, inserting your credentials as shown:

```sh
{
  "services": {
    "messages-for-rabbitmq-gen2": [
      {
        "credentials": INSERT YOUR CREDENTIALS HERE
      }
    ]
  }
}
```
{: .pre}

- Save the file as `vcap-local.json` in the directory where the sample app is located.

To avoid accidentally exposing your credentials when you push an application to GitHub or {{site.data.keyword.cloud_notm}}, make sure that the file that contains your credentials is listed in the relevant ignore file. If you open `.cfignore` and `.gitignore` in your application directory, you can see that `vcap-local.json` is listed in both. It is not included in the files that are uploaded when you push the app to either GitHub or {{site.data.keyword.cloud_notm}}.
{: .tip}

Now start the local server.
```sh
npm start
```
{: .pre}

The app is now running at `http://localhost:8080`. You can add words and definitions to your {{site.data.keyword.messages-for-rabbitmq}} database. When you stop and restart the app, any words you added are displayed when you refresh the page.

## Next steps
{: #next-steps}

To understand more about how the [sample app](https://github.com/IBM-Cloud/clouddatabases-helloworld-cloudfoundry-examples/tree/node/rabbitmq){: .external} works, you can read the application's readme file, or the code comments in `server.js`, which give some information about the app's functions.

To start exploring your {{site.data.keyword.messages-for-rabbitmq}} service, see the following topics about the service dashboard:

- [Dashboard overview](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-dashboard-overview)
- [Backups](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-backups-for-rabbitmq)
- [Creating users and getting connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings)
