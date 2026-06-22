---
copyright:
  years: 2026
lastupdated: "2026-06-22"

keywords: rabbitmq, databases, pricing, resources, scaling, rabbitmq pricing

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}

[Gen 2]{: tag-purple}

A {{site.data.keyword.messages-for-rabbitmq_full}} Gen 2 Standard plan deploys as one highly available RabbitMQ cluster with three data members on the Isolated Compute hosting model. The Standard plan is priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly.

Gen 2 offers two types of profiles: **Fixed profiles** (newest compute generation, consistent performance) and **Flex profiles** (across CPU generations, cost-optimized). All deployments use Isolated Compute with dedicated resources.



## Using the pricing calculator
{: #using-pricing-calc}

Templates are provided for ease of use and provide balanced resource allocations appropriate for general-purpose workloads. 

For pricing estimation, use the **Add to estimate** button at the bottom of the [{{site.data.keyword.messages-for-rabbitmq}} catalog page](https://cloud.ibm.com/databases/messages-for-rabbitmq-gen2/create?catalog_query). Input your total consumption across three data members into the calculator. For example, 1 GB of disk and 1 GB of RAM across three data members would be priced at 3 GB of disk and 3 GB of RAM respectively.

## Backups pricing
{: #backups-pricing}

Users also receive their total disk space purchased, per database, in free backup storage. For example, in a month, if you have a {{site.data.keyword.messages-for-rabbitmq}} deployment that has provisioned 10 GB of disk per member, which has three data members, you receive 30 GB of backup storage free for that month. If your backup storage utilization is greater than 30 GB for the month in this scenario, each gigabyte is charged at an overage $0.095/month. 

## Dedicated cores pricing
{: #cores-pricing}

On Gen 2, all deployments use isolated compute with dedicated CPU cores.  The number of CPUs that are allocated to your deployment depends on the profile size that you select.

Gen 2 offers Fixed and Flex profiles with various CPU and RAM configurations. 

Shared Compute is not available on Gen 2. All deployments use Isolated Compute with dedicated resources.
{: note}

## Scaling per member
{: #scaling-member}

Scaling a {{site.data.keyword.messages-for-rabbitmq_full}} compute or disk is currently unavailable. It will be made available soon.
{: important}
