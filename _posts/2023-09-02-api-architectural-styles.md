---
title: "API architectural styles"
author: Amin Ziagham
date: 2023-09-02
last_modified_at: 2023-09-02
categories: [Software Architecture]
tags: [API, Software Architecture, Architecture Patterns]
description: There are various ways to communicate between two systems (devices, web applications, etc.) made possible by APIs. Therefore, if you plan to connect ...
image: '/assets/img/posts/2023/09/APIsArchitectural-OG.jpg'
---

Today, Application Programming Interfaces (APIs) are the main axis in the software ecosystem. There are various ways to communicate between two systems (devices, web applications, etc.) made possible by APIs. Therefore, if you plan to connect your application or service to the digital world, understanding how APIs work and the types of APIs is essential. The important thing to keep in mind is that not all APIs are the same and have different uses depending on the type of access and architecture structure. It doesn't matter if you're an API developer or an API user, you need to know what kind of API you need. For example, an API that provides data to the public has a different structure than an API that is designed to exchange data within an organization. In this article, we'll go over the different types of APIs so you know which one is right for you and your business.

## Types of APIs in terms of access level
The meaning of API is actually one of its subcategories called Web API. Web APIs are a type of API that can be accessed through the HTTP protocol (short for Hypertext Transfer Protocol). The HTTP protocol is used to receive and display information on web pages. We can categorize APIs based on access level and target audience. There are four general types of APIs:
- Public APIs
- Partner APIs
- Internal APIs
- Composite APIs

### Public APIs
These types of APIs, which are also called Open APIs or External APIs, are available to all developers and businesses. As a result, public APIs have a low level of authentication and are limited in the data that can be provided. Some of these APIs are free and others require a subscription or pay-per-use fee. Developing public APIs has many benefits for a business, the most important of which is open access to information. In this way, businesses and developers are encouraged to use the API in their service or application, and the value of the API and that application increases. The low level of authentication and ease of use also allow businesses to use data provided by public APIs more flexibly. For example, map services can display information about road traffic, road blockages, and accidents using free city traffic APIs. In this way, on the one hand, the map service makes commuting in the city easier, and on the other hand, traffic management by the municipality is improved.

### Partner APIs
These types of APIs are only available to businesses that have a business relationship with the API provider or sign a contract to use the API. The security level of these APIs is higher than public APIs and only authorized systems have access to it. Businesses use collaborative APIs for two reasons:

- Control who has access to the API
- Resource and data control

For example, the Pinterest platform has a request form in which API applicants must describe the details of how to use the API.

### Internal APIs
Unlike the previous two types, internal APIs (or private APIs) are not designed for use by businesses and third parties. These APIs are designed only for use within organizations and companies, and their task is to facilitate the exchange of data between teams and different parts of the company. Internal APIs are hidden from public view due to lack of documentation by public documentation software (or lack of documentation at all).

The features of APIs such as security, optimization, scalability, and traceability make it a suitable option for exchanging information within organizations.

### Composite APIs
This model of APIs combines several APIs together and allows developers to receive a comprehensive response from different servers by submitting a combination of different requests. If you need data from multiple applications and data sources, you should use the Composite API. Also, you can take advantage of the Composite API to create an automated chain of requests and responses without manual intervention.

Composite APIs reduce server load by reducing the number of requests and produce faster and simpler systems. For example, in a shopping cart API, an API to create a user profile, an API to create an order, an API to add products and an API to change the order status are needed. Now, instead of developing several separate APIs for each of these services, it is enough to create a Composite API.

{% include advertisements.html %}

## Types of architecture APIs
Another way to categorize APIs is by architectural structure. The architecture of an API includes the rules and guidelines that dictate what information an API can share and how. The three types of **SOAP**, **REST**, **GraphQL**, **gRPC**, **WebSocket**, **WebHook**, **MQTT**, and **AMQP** are the most popular API architectures, which we describe below.

### SOAP (Simple Object Access Protocol)
Simple Object Access Protocol (SOAP) is an official API protocol. The World Wide Web Consortium (W3C) maintains the SOAP protocol, which is one of the earliest API architectures. Its design makes it easy to communicate between applications built with different languages and platforms. The SOAP format describes an API using Web Service Description Language (WSDL). It is written in Extensible Markup Language (XML). This format imposes internal conformance standards that increase security, stability, isolation, and durability. These features ensure reliable database transactions that make SOAP better for enterprise development.

When a user requests content through a SOAP API, it goes through standard layer protocols. The response is in human and machine readable XML format. Like REST APIs, SOAP APIs do not cache/store information. If you need the data later, you must submit another request. **SOAP supports stateful and stateless data exchange**.

### REST (Representational State Transfer)
REST APIs are modern and the most popular API architecture used by developers. REST (Representational State Transfer) is an architecture used to design client-server applications. It is not a protocol or standard, so you can implement it in different ways. This aspect increases your flexibility as a developer. REST provides access to requested data stored in a database. You can perform basic CRUD functions with a REST API. When clients request content through a RESTful API, they must use the appropriate headers and parameters. Headers contain useful metadata to identify a resource, such as status and authorization codes.

The information transferred via HTTP can be in the form of JSON, HTML, XML or plain text. JSON is the most common file format used for REST APIs. JSON is an anonymous and human-readable language.

### GraphQL
GraphQL is a query language for an API. It is a server-side runtime that executes queries based on a defined set of data. GraphQL has specific use cases. Its architecture allows you to declare the specific information you need. Unlike the REST architecture, where HTTP handles client requests and responses, GraphQL requests data by query. A GraphQL service defines types and the fields of those types, then provides functions for each field and type.

This service receives GraphQL queries for validation and execution. First, it checks a query to make sure it refers to the defined types and fields. Then, it executes the associated functions to produce the desired result.

### gRPC (Google Remote Procedure Call)
gRPC is a powerful open source framework designed based on Remote Procedure Call (RPC) and can be implemented in all development environments. This technology provides the possibility of clear, convenient, and bi-directional communication and coordination between the client and the server and also simplifies the construction of connected systems. This technology is design based on the HTTP/2. One of the interesting features of this technology is the connection between services and all data centers (Data Center) with the ability to trace (Tracing), load Balancing (Load Balancing), checking the level of access and health of the service mentioned. This framework also has the ability to connect mobile and browser to back-end services.

### WebSocket
WebSocket is a TCP-based protocol used for communication between a user's browser and a server. This protocol allows programmers to transfer data between the browser and the server in a two-way (Full-Duplex) manner and without the need to make repeated requests. WebSocket is supported in all modern browsers and is one of the most basic communication technologies in the field of web application development.

By using WebSocket, it is possible to communicate between the browser and the server to transfer data and information in a stable manner. In other words, the WebSocket protocol allows programmers to transfer data permanently between the browser and the server without having to resend requests. By using this web feature, you can take more advantage of the communication between applications. This protocol is used for many web applications such as chat programs, online games, regular updates, etc. is used

### WebHook
A webhook is an HTTP callback that is generated by an event on the source system and is often sent to the destination system along with a volume of data (payload). Webhooks are automated. In other words, they are sent automatically when their event is triggered on the source system. Webhook provides a method for a system (source) to communicate with another system (destination) when an event occurs and share event information.

### MQTT (Message Queuing Telemetry Transport)
MQTT communication is a simple messaging protocol designed for constrained devices, high-latency networks, and low-bandwidth devices. Therefore, it is a suitable data exchange solution for machine-to-machine (M2M) communication and plays an important role in the Internet of Things (IoT). MQTT communication works as a publish and subscribe system. Devices broadcast messages about a specific topic. All devices that subscribe to that topic receive the message.

One of its main uses is sending messages to control outputs, reading and publishing data from sensors connected to the Internet.

### AMQP (Advanced Message Queuing Protocol)
AMQP or Advanced Message Queuing Protocol is an open standard for transferring messages between systems or APIs. This protocol connects the systems to each other and also brings the business processes to their goals according to the information they need through reliable instructions. It offers features like message orientation, queuing, routing, reliability, and security. Following this standard, middleware written in different languages can send messages to each other.

## Conclusion
There are various ways to communicate between two systems (devices, web applications, etc.) made possible by APIs. Choosing the best API architectural style is not an easy task. The style you choose can have a big impact on the success of your application. It is critical to the application's performance, efficiency, security, and scalability. Each architectural style has its own set of capabilities and considerations, whether it be SOAP, REST, GraphQL, gRPC, WebSocket, WebHook, MQTT, or AMQP. 