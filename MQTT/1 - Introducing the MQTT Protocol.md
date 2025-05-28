## Introduction to MQTT
>“MQTT is a Client Server publish/subscribe messaging transport protocol. It is light weight, open, simple, and designed so as to be easy to implement. These characteristics make it ideal for use in many situations, including constrained environments such as for communication in Machine to Machine (M2M) and Internet of Things (IoT) contexts where a small code footprint is required and/or network bandwidth is at a premium.“

_Citation from the official MQTT 3.1.1 specification_


The abstract of the MQTT specification does a good job describing what MQTT is all about. It is a very light weight and binary protocol, and due to its minimal packet overhead, MQTT **excels when transferring data over the wire in comparison to protocols like HTTP**. Another important aspect of the protocol is that MQTT is extremely easy to implement on the client side. Ease of use was a key concern in the development of MQTT and makes it a perfect fit for constrained devices with limited resources today.

## A little bit of history

The MQTT protocol was invented in 1999 by Andy Stanford-Clark (_IBM_) and Arlen Nipper (_Arcom, now Cirrus Link_). They needed a protocol for minimal battery loss and minimal bandwidth to connect with oil pipelines via satellite. The two inventors specified several requirements for the future protocol:

* Simple implementation
* Quality of Service data delivery
* Lightweight and bandwidth efficient
* Data agnostic
* Continuous session awareness

These goals are still at the core of MQTT. However, the primary **focus of the protocol has changed from proprietary embedded systems to open Internet of Things (IoT) use cases.** This shift in focus has created a lot of confusion about what the acronym MQTT stands for. The short answer is that **MQTT is no longer considered an acronym. MQTT is simply the name of the protocol.**

The longer answer is that the former acronym stood for MQ Telemetry Transport.

“MQ” refers to the MQ Series, a product IBM developed to support MQ telemetry transport. When Andy and Arlen created their protocol in 1999, they named it after the IBM product. Many sources label MQTT incorrectly as a message queue protocol. That is simply not true. MQTT is not a traditional message queuing solution (although it is possible to queue messages in certain cases, a fact that we discuss in detail in an upcoming post). Over the next ten years, IBM used the protocol internally until they released MQTT 3.1 as a royalty-free version in 2010. Since then, everyone is welcome to implement and use the protocol.


## OASIS Standard and current version

Approximately 3 years after the initial publication, it was announced that MQTT would be standardized under the wings of OASIS, an open organization with the purpose of advancing standards. AMQP, SAML,and DocBook are just a few of the previously released OASIS standards. The standardization process took around 1 year. On October 29, 2014 **MQTT became an officially approved OASIS Standard.** The minor version change from 3.1 to 3.1.1 shows that few changes were made to the previous version.