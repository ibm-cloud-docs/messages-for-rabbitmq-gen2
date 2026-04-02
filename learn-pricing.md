---
copyright:
  years: 2026, 2023
lastupdated: "2023-03-03"

keywords: rabbitmq, databases, pricing, resources, scaling, rabbitmq pricing

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}


[Gen 2]{: tag-purple}

A {{site.data.keyword.messages-for-rabbitmq-gen2_full}} Standard plan deploys as one highly available RabbitMQ cluster with three data members. Your data is replicated across members. The Standard plan is priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.messages-for-rabbitmq-gen2}} deployments have a minimum of 1 GB of disk and 1 GB of RAM per data member.

## Using the pricing calculator
{: #using-pricing-calc}

Templates are provided for ease of use and provide balanced resource allocations appropriate for general-purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as wanted.

For pricing estimation, use the **Add to estimate** button at the bottom of the [{{site.data.keyword.messages-for-rabbitmq-gen2}} catalog page](https://cloud.ibm.com/databases/messages-for-rabbitmq-gen2/create?catalog_query). Input your total consumption across three data members into the calculator. For example, 1 GB of disk and 1 GB of RAM across three data members would be priced at 3 GB of disk and 3 GB of RAM respectively.

## Backups pricing
{: #backups-pricing}

Users also receive their total disk space purchased, per database, in free backup storage. For example, in a month, if you have a {{site.data.keyword.messages-for-rabbitmq-gen2}} deployment that has provisioned 1 GB of disk per member, which has three data members, you receive 3 GB of backup storage free for that month. If your backup storage utilization is greater than 3 GB for the month in this scenario, each gigabyte is charged at an overage $0.03/month. Most deployments will not ever go over the allotted credit.

## Dedicated cores pricing
{: #cores-pricing}

You have the option of selecting the CPU allocation for your deployment. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $30 per core per month, and each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member, that is a total of 9 cores, and billed at $270 per month.

Dedicated cores are an optional feature. The default `Shared CPU` setting provisions your deployment on hosts with shared compute resources and incurs no additional charge.

## Scaling per member
{: #scaling-member}

{{site.data.keyword.messages-for-rabbitmq-gen2}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.

| Resource | Minimum | Maximum | Scaling granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 5 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 1 GB per member | 112 GB per member | 128 MB per member |
| CPU (if enabled) | 3 CPUs per member | 28 CPUs per member| 1 CPU per member |
{: caption="Per Member Scaling Limits" caption-side="top"}
