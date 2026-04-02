---
copyright:
  years: 2026
lastupdated: "2024-02-12"

keywords: rabbitmq, streams

subcollection: messages-for-rabbitmq-gen2

---

{{site.data.keyword.attribute-definition-list}}

# RabbitMQ Streams
{: #rabbitmq-streams}


[Gen 2]{: tag-purple}

RabbitMQ Streams is a persistent replicated data structure that functions similarly to queues, buffering messages from producers for consumption by consumers. However, streams differ in two ways:
- How producers write messages to them.
- How consumers read messages from them.

Streams model an append-only log of messages that can be repeatedly read until they expire. Streams are always persistent and replicated. A more technical description of this stream behavior is “nondestructive consumer semantics."

To read messages from a stream in RabbitMQ, one or more consumers subscribe to it and read the same message as many times as they want. Consumers talk to a stream through AMQP-based clients and use the AMQP protocol.

## Use Cases
{: #rabbitmq-streams-use-cases}

The use cases for streams include:

- Fan-out architectures: Where many consumers need to read the same message.
- Replay and time-travel: Where consumers need to read and reread the same message or start reading from any point in the stream.
- Large backlogs: Streams are designed to store larger amounts of data efficiently with minimal in-memory overhead.
- High Throughput: RabbitMQ Streams processes relatively higher volumes of messages per second.

For more information, see [RabbitMQ Streams](https://www.rabbitmq.com/streams.html){: external}.

## How to Use RabbitMQ Streams
{: #rabbitmq-streams-howto}

An AMQP 0.9.1 client library that can specify [optional queue and consumer arguments](https://www.rabbitmq.com/queues.html#optional-arguments){: external} is able to use streams as regular AMQP 0.9.1 queues.

### Using with an AMQP Client Library
{: #rabbitmq-streams-howto-amqp-client-lib}

There are three steps to working with RabbitMQ Streams through an AMQP client library:
1. [Declare/Instantiate a stream](#declaring-a-rabbitmq-stream)
1. [Publish (write) messages to the stream](#publishing-a-rabbitmq-stream)
1. [Consume (read) messages from the stream](#consuming-a-rabbitmq-stream)

#### Declaring a RabbitMQ Stream
{: #rabbitmq-streams-howto-declare}

You can create a stream by using the RabbitMQ Management Interface.
- First, select **Add a new queue**.
- Next, in the *Type* dropdown, select *Stream*.

Alternatively, create a stream by using the RabbitMQ Management Interface by creating a queue with type `“stream”`. If a queue is already present, it will not be created. Declare a stream with a command like:

```sh
import pika, os

credentials = pika.PlainCredentials('username', 'password')
context = ssl.SSLContext(ssl.PROTOCOL_SSLv23)
context.verify_mode = ssl.CERT_NONE
ssl_options = pika.SSLOptions(context)

# Connection
connection = pika.BlockingConnection(
  pika.ConnectionParameters(
    host='<hostname>',
    port='<port>',
    credentials=credentials,
    virtual_host="/",
    ssl_options=ssl_options))
channel = connection.channel() # start a channel

# Declare a Stream, named test_stream
channel.queue_declare(
  queue='test_stream',
      durable=True,
  arguments={"x-queue-type": "stream"}
)
```
{: codeblock}

#### Publishing a RabbitMQ Stream
{: #rabbitmq-streams-howto-publish}

The following script declares a RabbitMQ stream (`test_stream`) then publishes a message to it through the `basic_publish` function:

```sh
import pika, os

credentials = pika.PlainCredentials('username', 'password')
context = ssl.SSLContext(ssl.PROTOCOL_SSLv23)
context.verify_mode = ssl.CERT_NONE
ssl_options = pika.SSLOptions(context)

# Connection
connection = pika.BlockingConnection(
  pika.ConnectionParameters(
    host='<hostname>',
    port='<port>',
    credentials=credentials,
    virtual_host="/",
    ssl_options=ssl_options))
channel = connection.channel() # start a channel

# Declare a Stream, named test_stream
channel.queue_declare(
  queue='test_stream',
      durable=True,
  arguments={"x-queue-type": "stream"}
)

# Publish a message to the test_stream
channel.basic_publish(
  exchange='',
  routing_key='test_stream',
  body='Welcome email message'
)
```
{: codeblock}

#### Consuming a RabbitMQ Stream
{: #rabbitmq-streams-howto-consume}

Messages can be consumed from a stream the same way queues accomplish this task, with two major differences:

1. Consuming messages in RabbitMQ Streams requires setting the [QoS prefetch](https://www.rabbitmq.com/consumer-prefetch.html){: external}.
1. Specify an offset to start reading/consuming from any point in the log stream. If unspecified, the consumer starts reading from the most recent offset written to the log stream after it starts.

The following script declares `test_stream` again, setting the QoS prefetch to `100` using the `basic_qos` function. The consumer triggers the callback function when it processes a new message. The callback function then invokes the `send_welcome_email` function that simulates sending an email to a user.

```sh
import pika, os, time

def send_welcome_email(msg):
  print("Welcome Email task processing")
  print(" [x] Received " + str(msg))
  time.sleep(5) # simulate sending email to a user --delays for 5 seconds
  print("Email successfully sent!")
  return

credentials = pika.PlainCredentials('username', 'password')
context = ssl.SSLContext(ssl.PROTOCOL_SSLv23)
context.verify_mode = ssl.CERT_NONE
ssl_options = pika.SSLOptions(context)

# Connection
connection = pika.BlockingConnection(
  pika.ConnectionParameters(
    host='<hostname>',
    port='<password>',
    credentials=credentials,
    virtual_host="/",
    ssl_options=ssl_options))
channel = connection.channel() # start a channel

# Declare our stream
channel.queue_declare(
  queue='test_stream',
  durable=True,
  arguments={"x-queue-type": "stream"}
)

# create a function which is called on incoming messages
def callback(ch, method, properties, body):
  send_welcome_email(body)

# Set the consumer QoS prefetch
channel.basic_qos(
  prefetch_count=100
)

# Consume messages published to the stream
channel.basic_consume(
  'test_stream',
  callback,
)

# start consuming (blocks)
channel.start_consuming()
connection.close()
```
{: codeblock}

Note how an offset isn’t specified in our `basic_consume`: `# Consume messages published to the stream channel.basic_consume( 'test_stream', callback)`. As a result, the consumer starts reading from the most recent offset written to `test_stream` after the consumer starts. *After* has been deliberately emphasized here to allow for the cross-examination of an interesting behavior of streams.

### How to set an offset
{: #rabbitmq-streams-howto-consume-offset}

As streams never delete any messages, any consumer can start reading/consuming from any point in the log. This is controlled by the `x-stream-offset` consumer argument. If it is unspecified, the consumer will start reading from the next offset written to the log after the consumer starts. The following values are supported:

- first - start from the first available message in the log
- last - this starts reading from the last written "chunk" of messages
- next - same as not specifying any offset
- Offset - a numerical value specifying an exact offset to attach to the log at.
- Timestamp - a timestamp value specifying the point in time to attach to the log at. It clamps to the closest offset, if the timestamp is out of range for the stream it clamps either the start or end of the log, for example: 00:00:00 UTC, 1970-01-01. Be aware that consumers can receive messages published a bit before the specified timestamp.
- Interval - a string value specifying the time interval relative to current time to attach the log at. Uses the same specification as `x-max-age`.

The following code example shows how to use the first offset specification:

```sh
# Grabbing the first message
channel.basic_consume(
  'test_stream',
  callback,
  arguments={"x-stream-offset": "first"}
)
```
{: pre}

This code shows how to specify a specific offset to consume from:

```sh
channel.basic_consume(
  'test_stream',
  callback,
  arguments={"x-stream-offset": 5000}
)
```
{: pre}

## Other Streams Operations
{: #rabbitmq-streams-howto-other}

The following operations can be used in a similar way to classic and quorum queues but some have some queue-specific behavior:
- [Declaration](https://www.rabbitmq.com/streams.html#declaring){: external}
- Queue deletion
- [Publisher confirms](https://www.rabbitmq.com/confirms.html#publisher-confirms){: external}
- [Consumption](https://www.rabbitmq.com/consumers.html){: external} (subscription): consumption requires QoS prefetch to be set. The acks works as a credit mechanism to advance the current offset of the consumer.
- Setting [QoS prefetch](https://www.rabbitmq.com/streams.html#global-qos){: external} for consumers
- [Consumer acknowledgments](https://www.rabbitmq.com/confirms.html){: external} (keep [QoS Prefetch Limitations](https://www.rabbitmq.com/streams.html#global-qos){: external} in mind)
- Cancellation of consumers
