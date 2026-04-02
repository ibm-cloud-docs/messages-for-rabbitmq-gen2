---

copyright:
  years: 2026, 2026
lastupdated: "2026-04-02"

keywords: troubleshooting Messages for RabbitMQ, connectivity, definitions, error importing definitions

subcollection: messages-for-rabbitmq-gen2

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I import definitions between {{site.data.keyword.messages-for-rabbitmq-gen2}} versions?
{: #troubleshoot-defs}

[Gen 2]{: tag-purple}

{: troubleshoot}
{: support}

If you encounter errors while importing definitions between {{site.data.keyword.messages-for-rabbitmq-gen2}} versions, review these common causes and solutions.
{: shortdesc}

You encounter errors while importing definitions between {{site.data.keyword.messages-for-rabbitmq-gen2}} versions.
{: tsSymptoms}

[RabbitMQ definitions](https://www.rabbitmq.com/definitions.html){: external} are metadata that RabbitMQ stores about its cluster. This metadata includes information about users, vhosts, queues, exchanges, bindings, and runtime parameters. Definitions can be used to restore a cluster or migrate to a new cluster. An error while importing definitions can be because of invalid imported arguments being imported from {{site.data.keyword.messages-for-rabbitmq-gen2}} versions. Review the following information to troubleshoot and resolve common definition problems:
{: tsResolve}

Look into your [logs](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-logging) and search for this line:

```sh
Encountered an error when importing definitions
```
{: pre}

If this error is found, check which argument is causing the issue. When you have identified the argument causing the issue, correct it and try to import the definition again.

## How can I restore the backup if my instances have temporary or server-named queues?
{: #troubleshoot-queues}

Users will encounter an error while restoring with temporary queues. RabbitMQ can create temporary queues if those are not declared. These queues start with prefix "amq.gen-". These temporary queues are exported in the definition of the backup, and RabbitMQ attempts to restore them. However, RabbitMQ does not allow temporary queue definitions to be imported, which will fail the restore when exchange binding is created.

For a successful restore (import) of the backup, users **must** remove the temporary queues from their instances before they create a backup (export).

RabbitMQ allows you to export and import definitions, which are configurations for exchanges, users, permissions, and non-transient queues.
{: note}

Read more about temporary [server-named queues](https://www.rabbitmq.com/docs/queues#server-named-queues){: external}.
