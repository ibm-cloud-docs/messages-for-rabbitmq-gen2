---

copyright:
  years: 2026,
lastupdated: "2026-06-22"

keywords: data export, portability

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}



# Understanding data portability for {{site.data.keyword.messages-for-rabbitmq}}
{: #data-portability}

[Gen 2]{: tag-purple}

[Data portability](#x2113280){: term} involves a set of tools, and procedures that enable customers to export the digital artifacts that would be needed to implement similar workload and data processing on different service providers or on-prem software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

IBM Cloud services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer then is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures.
This can involve the following:

- Planning and execution for setting up alternate infrastructure on on different cloud providers or on-prem software that provide similar capabilities to the IBM services.
- Planning and execution for the porting of the required application code on the alternate infrastructure, including the adaptation of customer's application code, and deployment automation.
- Conversion of the exported data and configuration to format required by the alternate infrastructure and adapted applications.

For more information about your responsibilities when using {{site.data.keyword.messages-for-rabbitmq_full}}, see [Shared responsibilities for {{site.data.keyword.databases-for}}](/docs/cloud-databases?topic=cloud-databases-responsibilities-cloud-databases).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.messages-for-rabbitmq}} provides mechanisms to export your data that has been uploaded, stored, and processed using the service.

### Exporting data
{: #data-portability-exporting-data}

You can create replication relationship between brokers and export the messages from {{site.data.keyword.messages-for-rabbitmq}} using [RabbitMQ Shovel](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-shovel&interface=ui). Note that the term *formation* refers to *service instance*.

## Exported data formats
{: #data-portability-data-formats}

The format of the data exported from the {{site.data.keyword.messages-for-rabbitmq}} remains unchanged.

## Data ownership
{: #data-ownership}

All exported data are classified as Customer content and therefore apply to them the full customer ownership and licensing rights, as stated in [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS){: external}.
