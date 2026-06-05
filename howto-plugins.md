---
copyright:
  years: 2026,
lastupdated: "2026-06-05"

keywords: rabbitmq, databases, jms, shovel, delayed, stomp, mqtt, consistent hash exchange, rabbitmq plug-in

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Using RabbitMQ plug-ins
{: #plug-ins}

[Gen 2]{: tag-purple}

RabbitMQ supports various plug-ins to extend its core functions. {{site.data.keyword.messages-for-rabbitmq_full}} comes with a selection of plug-ins that are already enabled on your deployment. Plug-in management is handled by the {{site.data.keyword.messages-for-rabbitmq}} service. Users cannot enable or disable plug-ins.

## Available {{site.data.keyword.messages-for-rabbitmq}} plug-ins
{: #plug-ins-available}

| Plug-in | Name  |
| ------- | ------- |
| [Shovel plug-in](#shovel-plug-in) | `rabbitmq_shovel`  |
| [Shovel Management plug-in](#delayed-message-plug-in)| `rabbitmq_shovel_management` |
| [Delayed Message plug-in](#delayed-message-plug-in) | `rabbitmq_delayed_message_exchange` |
| [STOMP plug-in](#stomp-plug-in) | `rabbitmq_stomp` |
| [RabbitMQ Management plug-in](#rabbitmq-management-plug-in) | `rabbitmq_management` |
| [JMS plug-in](#jms-plug-in) | `rabbitmq_jms_topic_exchange` |
| [MQTT plug-in](#mqtt-plug-in) | `rabbitmq_mqtt` |
| [RabbitMQ Consistent Hash Exchange Type plug-in](#consistent-hash-exchange-type-plug-in) | `rabbitmq_consistent_hash_exchange` |
{: caption="Available RabbitMQ plug-ins" caption-side="top"}


## Shovel plug-in
{: #shovel-plug-in}

{{site.data.keyword.messages-for-rabbitmq}} comes with the [Shovel plug-in](https://www.rabbitmq.com/shovel.html), `rabbitmq_shovel`, and the [Shovel Management plug-in](https://github.com/rabbitmq/rabbitmq-shovel-management), `rabbitmq_shovel_management`, enabled. You can configure the plug-in through both the [RabbitMQ Management plug-in UI](#rabbitmq-management-plug-in) and its accompanying API.

The _Shovel Status_ and _Shovel Management_ links appear under the _Admin_ tab.

![Admin tab with the Shovel plug-in](images/plugins-shovel-ui.png){: caption="Admin tab with the Shovel plug-in" caption-side="bottom"}

If you do not see the _Admin_ tab, you might need to log in to the management UI with the [admin user](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-user-management&interface=ui#user-management-set-admin-password-ui).
{: .tip}

In the RabbitMQ Management API, the Shovel Management plug-in adds endpoints for listing, creating, and deleting shovels. Usage and examples are in the [GitHub repository's documentation](https://github.com/rabbitmq/rabbitmq-shovel-management#usage).

## Delayed Message plug-in
{: #delayed-message-plug-in}

The [delayed message plug-in](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange){: .external}, `rabbitmq_delayed_message_exchange` is a third-party plug-in that adds delayed messaging and scheduled messaging to RabbitMQ.

To use the plug-in, declare an exchange with the type `x-delayed-type`. To delay a message, publish it with the `x-delay` header with the number of milliseconds to delay the message. More detailed usage information is in the [plug-in's documentation](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange#usage){: .external}.

## STOMP plug-in
{: #stomp-plug-in}

The [RabbitMQ STOMP plug-in](https://www.rabbitmq.com/stomp.html){: .external}, `rabbitmq_stomp`, supports sending [STOMP-formatted](http://stomp.github.io/){: .external} messages through RabbitMQ. The plug-in enables a port to handle STOMP traffic on your deployment, and it is TLS/SSL secured. The connection information for STOMP clients is in the `stomp_ssl` section of your deployment's [connection strings](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-connection-strings#the-stomp_ssl-section).

## RabbitMQ Management plug-in
{: #rabbitmq-management-plug-in-info}

The RabbitMQ Management plug-in, `rabbitmq_management`, provides access to your deployment through a web browser, `rabbitmqadmin`, and through the RabbitMQ API. For more information, see [Connecting with the RabbitMQ Management plug-in](/docs/messages-for-rabbitmq-gen2?topic=messages-for-rabbitmq-gen2-rabbitmq-management-plugin).

## JMS plug-in
{: #jms-plug-in}

The RabbitMQ JMS Topic Exchange plug-in, `rabbitmq_jms_topic_exchange`, is enabled by default for {{site.data.keyword.messages-for-rabbitmq}} deployments. This plug-in adds server-side support for the [RabbitMQ JMS client](https://github.com/rabbitmq/rabbitmq-jms-client){: .external} and supports JMS topic routing plus selection based on JMS SQL selection rules.

The RabbitMQ JMS Client is required to communicate with the plug-in. Review the [JMS Client documentation page](https://www.rabbitmq.com/jms-client.html){: .external} that is provided by RabbitMQ for more detailed information on installation and configuration, along with examples.

More detailed information about the JMS plug-in can also be found in the RabbitMQ JMS Topic Exchange plug-in [GitHub repo](https://github.com/rabbitmq/rabbitmq-jms-topic-exchange){: .external}.

## MQTT plug-in
{: #mqtt-plug-in}

The MQTT plug-in, `rabbitmq_mqtt`, is enabled by default for {{site.data.keyword.messages-for-rabbitmq}} deployments. Information on using the MQTT plug-in is available on the [MQTT plug-in](https://www.rabbitmq.com/mqtt.html){: .external} page.

## RabbitMQ Consistent Hash Exchange Type plug-in
{: #consistent-hash-exchange-type-plug-in}

The RabbitMQ plug-in, `rabbitmq_consistent_hash_exchange`, is enabled by default for {{site.data.keyword.messages-for-rabbitmq}} deployments. This plug-in adds a consistent-hash exchange type to RabbitMQ. Information on using the Consistent Hash Exchange plug-in is available in the [consistent_hash_exchange plug-in readme](https://github.com/rabbitmq/rabbitmq-server/blob/main/deps/rabbitmq_consistent_hash_exchange/README.md){: .external} page.
