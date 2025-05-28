## The publish/subscribe pattern

The publish/subscribe pattern (also known as pub/sub) provides an alternative to traditional client-server architecture. In the client-sever model, a client communicates directly with an endpoint.The pub/sub model **decouples the client that sends a message (the publisher) from the client or clients that receive the messages (the subscribers).** The publishers and subscribers never contact each other directly. In fact, they are not even aware that the other exists. **The connection between them is handled by a third component (the broker).** The job of the broker is to filter all incoming messages and distribute them correctly to subscribers.

<img src="img/mqtt-publish-subscribe.svg">

_MQTT Publish / Subscribe Architecture_


The most important aspect of pub/sub is the decoupling of the publisher of the message from the recipient (subscriber). This decoupling has several dimensions:

* **Space decoupling:** Publisher and subscriber do not need to know each other (for example, no exchange of IP address and port).
* **Time decoupling:** Publisher and subscriber do not need to run at the same time.
* **Synchronization decoupling:** Operations on both components do not need to be interrupted during publishing or receiving.
In summary, the pub/sub model removes direct communication between the publisher of the message and the recipient/subscriber. The filtering activity of the broker makes it possible to control which client/subscriber receives which message. The decoupling has three dimensions: space, time, and synchronization.


## Scalability

**Pub/Sub scales better than the traditional client-server approach.** This is because operations on the broker can be highly parallelized and messages can be processed in an event-driven way. Message caching and intelligent routing of messages are often a decisive factors for improving scalability. Nonetheless, scaling up to millions of connections is a challenge. Such a high level of connections can be achieved with clustered broker nodes to distribute the load over more individual servers using load balancers.

## Message filtering

It’s clear that the broker plays a pivotal role in the pub/sub process. But how does the broker manage to filter all the messages so that each subscriber receives only messages of interest? As you’ll see, the broker has several filtering options:

OPTION 1: SUBJECT-BASED FILTERING
This filtering is based on the subject or topic that is part of each message. The receiving client subscribes to the broker for topics of interest. From that point on, the broker ensures that the receiving client gets all message published to the subscribed topics. In general, topics are strings with a hierarchical structure that allow filtering based on a limited number of expressions.

OPTION 2: CONTENT-BASED FILTERING
In content-based filtering, the broker filters the message based on a specific content filter-language. The receiving clients subscribe to filter queries of messages for which they are interested. A significant downside to this method is that the content of the message must be known beforehand and cannot be encrypted or easily changed.

OPTION 3: TYPE-BASED FILTERING
When object-oriented languages are used, filtering based on the type/class of a message (event) is a common practice. For example,, a subscriber can listen to all messages, which are of type Exception or any sub-type.

**Of course, publish/subscribe is not the answer for every use case. There are a few things you need to consider before you use this model.** The decoupling of publisher and subscriber, which is the key in pub/sub, presents a few challenges of its own. For example, you need to be aware of how the published data is structured beforehand. For subject-based filtering, both publisher and subscriber need to know which topics to use. Another thing to keep in mind is message delivery. The publisher can’t assume that somebody is listening to the messages that are sent. In some instances, it is possible that no subscriber reads a particular message.


## MQTT

Now that we’ve explored the publish/subscribe model in general, let’s focus on MQTT specifically. Depending on what you want to achieve, **MQTT embodies all the aspects of pub/sub that we’ve mentioned:**

* MQTT decouples the publisher and subscriber spatially. To publish or receive messages, publishers and subscribers only need to know the hostname/IP and port of the broker.

* MQTT decouples by time. Although most MQTT use cases deliver messages in near-real time, if desired, the broker can store messages for clients that are not online. (Two conditions must be met to store messages: the client had connected with a persistent session and subscribed to a topic with a Quality of Service greater than 0).

* MQTT works asynchronously. Because most client libraries work asynchronously and are based on callbacks or a similar model, tasks are not blocked while waiting for a message or publishing a message. In certain use cases, synchronization is desirable and possible. To wait for a certain message, some libraries have synchronous APIs. But the flow is usually asynchronous.

Another thing that should be mentioned is that MQTT is especially easy to use on the client-side. Most pub/sub systems have the logic on the broker-side, but MQTT is really the essence of pub/sub when using a client library and that makes it a light-weight protocol for small and constrained devices.

**MQTT uses subject-based filtering of messages. Every message contains a topic (subject)** that the broker can use to determine whether a subscribing client gets the message or not.

To handle the challenges of a pub/sub system, **MQTT has three Quality of Service (QoS) levels.** You can easily specify that a message gets successfully delivered from the client to the broker or from the broker to a client. However, there is the chance that nobody subscribes to the particular topic. If this is a problem, the broker must know how to handle the situation. You can have the broker take action or simply log every message into a database for historical analyses. To keep the hierarchical topic tree flexible, it is important to design the topic tree very carefully and leave room for future use cases. **If you follow these strategies, MQTT is perfect for production setups.**

## Distinction from message queues

There is a lot of confusion about the name MQTT and whether the protocol is implemented as a message queue or not. We will try to shed some light on the topic and explain the differences. MQTT refers to the MQseries product from IBM and has nothing to do with “message queue“. Regardless of where the name comes from, it’s useful to understand the differences between MQTT and a traditional message queue:

**A message queue stores message until they are consumed** When you use a message queue, each incoming message is stored in the queue until it is picked up by a client (often called a consumer). If no client picks up the message, the message remains stuck in the queue and waits to be consumed. In a message queue, it is not possible for a message not to be processed by any client, as it is in MQTT if nobody subscribes to a topic.

**A message is only consumed by one client** Another big difference is that in a traditional message queue a message can be processed by one consumer only. The load is distributed between all consumers for a queue. In MQTT the behavior is quite the opposite: every subscriber that subscribes to the topic gets the message.

**Queues are named and must be created explicitly** A queue is far more rigid than a topic. Before a queue can be used, the queue must be created explicitly with a separate command. Only after the queue is named and created is it possible to publish or consume messages. In contrast, MQTT topics are extremely flexible and can be created on the fly.