---
layout: post
title:  "Zaqar What and Why"
date:   2015-07-28
tags:
  - openstack
  - messaging
  - zaqar
  - sqs
---

I added few lines of code in [Zaqar][3], [OpenStack][5] messaging and notification service as part of my Outreachy internship, but as the perfection is endless, learning and knowledge too. Zaqar developers are continuously adding more code to make Zaqar a perfect service, and I am learning more about Zaqar and other OpenStack components and continue contributing to OpenStack.

So as reading more about Zaqar and its use cases, I realized that if you look at the top level, will find it very similar to various other message brokers like [RabbitMQ][1], [ActiveMQ][2] etc. So what is the main difference between them and Zaqar, or it is yet another similar message broker? I think its worth to list all the benefits that Zaqar provide, and here it is.

### What is Zaqar?
Zaqar is a multi-tenant cloud messaging and notification service for OpenStack[[3]] that allow users to build scalable, reliable and high-performing applications. So if you want to send or receive messages between various components of your web or mobile applications Zaqar can help you. These components can be client device and server too.

### Why Zaqar?
Here are the benefits of Zaqar:  

#### The API
Zaqar provide a Fully HTTP based RESTful API, so it offer all the benefits that every other RESTfull API offer. Developer will have ease of using all Get, Put, Post, Delete etc HTTP calls for transforming messages. Developers can think in terms of resources and being HTTP driven, it is fully firewall-friendly.

The API is build for OpenStack, on the top of OpenStack, so it is integrated with other OpenStack components as much as possible. Like authentication layer of Zaqar is integrated with Keystone. Thus you can map the tenants present in keystone easily and serve multiple projects at the time. That is why Zaqar is multi-tenant cloud messaging service.

#### Scalability
Zaqar is designed not only to be easy to use for developers but also to be easy to scale. It is highly-available and horizontally scalable. In order to scale Zaqar service you only need to start another Zaqar server, which will be atomic to the rest of the applicaiton.

#### Fully Configurable
As like various other components of OpenStack Zaqar layers are fully pluggable, that is you can plug in and plug out the drivers you want to use. You can put all such information in conf file and configure the service the way you want. You can also configure default time_to_live for messages, maximum size of a message, maximum number of messages in a queue, max_reconnect_attempts and other such similar configs. You can configure the datastore you want to use for storing and managing messages. At present Zaqar support MongoDB and Redis. You can configure transport drivers to use as per your requirements and situation. At present Zaqar provide HTTP based and WebSocket protocol based transport drivers.

#### Persistence Transportation
As Zaqar support two types of transport driver, wsgi(HTTP bases) and websocket, you can choose the one which you want to use. If you are using wsgi driver for transporting your messages(which is the default one), that mean you are using simple HTTP calls to transport messages from point a to point b over the network. If you are using websocket transport driver, you can enjoy all those facilities that a wesocket transportation protocol provide like bi-directional, full-duplex communication, single TCP connection for client and server communicate for the lifecycle of WebSocket connection etc. You can read more about websocket vs HTTP at [[6]] and [[7]]. That mean you can send large messages in large amount efficiently with websocket protocol over the network. Thus you can persistently transport your messages safely if you are using Zaqar.

#### Multiple Messaging patterns
Zaqar support various [messaging patterns][4], like point to point communication, publisher-subscriber, producer-consumer. So you can use Zaqar as per the situation like if you want to use it for notification service, for event broadcasting, for task distributing or for any other pattern.

So these are all the benefits of using Zaqar!

[1]: http://rabbitmq.com/
[2]: http://activemq.apache.org/
[3]: https://wiki.openstack.org/wiki/Zaqar
[4]: https://wiki.openstack.org/wiki/Zaqar/Use_Cases
[5]: http://openstack.org/
[6]: http://enterprisewebbook.com/ch8_websockets.html
[7]: http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/
