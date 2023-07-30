---
title: Event Driven Architecture Pattern
author: Amin Ziagham
date: 2023-07-01
categories: [Software Architecture]
tags: [C#, .NET, Event Driven, Software Architecture]
description: Event-driven architecture is a software design pattern that uses events to determine the flow of data in an application. Events can be user actions...
image: '/assets/img/posts/2023/07/EDA-OG.jpg'
---

The request-response model in modern IT systems can become fairly complex when new applications and services are introduced to the infrastructure. To begin, if you want to verify that all elements are linked, you must define a request and a response for each interaction. For example, if Service A requires data from Service B, it will submit a request to Service B. Once Service B has processed the request and obtained the data, it will return the requested data to Service A. It is obvious that as the number of services provided by your system increases, you must likewise support all conceivable connections, which expand exponentially in relation to the number of services supported. Look at the Figure 1.

{% include image.html url="/assets/img/posts/2023/07/PossibleConnections.jpg" description="Possible connection between services" title="Figure-1: Possible connection between services" %}

When only a few services are involved, this process works well, but it quickly becomes unmanageable as the number of services increases. Furthermore, each service must be able to handle requests from other services in the system, making it difficult to add new services or update current ones without disrupting the entire system. **Event-Driven Architecture (EDA)** is an alternative technique that can aid in the simplification of modern software systems' complexity.

## What is Event-Driven Architecture (EDA)
Event-driven architecture is a software design pattern that uses events to determine the flow of data in an application. Events can be user actions, system alerts, or any other type of event that triggers an action in the application. In an event-driven architecture, applications are divided into smaller, independent components called services. In an event-driven architecture, when a service performs a part of the work that other services may benefit from, that service generates an event (a record of the action performed) and then other services receive that event and, then, they perform any task that requires the result of the event. Unlike REST, services that generate events do not need to know the details of services that consume events.

Events can be published in different ways. For example, they can be published to a queue that ensures delivery of the event to the appropriate consumers, or they can be published to a stream in a "pub/sub" model that publishes the event and provides access to all interested services. In any case, the producer releases the event and the consumer receives that event and reacts accordingly. Note that in some cases these two actors can also be called **Producer** (publisher) and **Consumer** (subscriber).

The figure 2, below, illustrates a simple view of a event-driven architecture.

{% include image.html url="/assets/img/posts/2023/07/EventDriven-Architecture.jpg" description="EventDriven Architecture" title="Figure-2: EventDriven Architecture" %}

The various components of this architecture are:
- **Event**: An event is a record of what has occurred and happened. They are immutable (they cannot be changed or deleted). This can be as simple as a user clicking button, an IoT sensor reading, or a system errors. A single event is only created once, but it can be consumed by numerous event consumers.
- **Event Producer**: An event producer is anything that can create an event. In most cases, event generators are applications or services.
- **Event Consumer**: An event consumer is anything that can consume an event in order to perform some kind of action. Services and applications can both be event consumers.
- **Event Broker**: An event broker is a piece of software that serves as a central communication hub for event producers and consumers. It is in charge of routing events to the appropriate consumers. There are many event brokers, such as rabbitmq, kafka, service bus, etc.

## Event-Driven Architecture Use Cases
Softwre architect leverage Event-driven architecture when designing their systems to achieve many different use cases. The following are the most common:
- **Parallel processing:** Multiple processes can be triggered by a single event and run asynchronously.
- **Data replication:** A single event can be shared by numerous services that require the data to be copied into their databases.
- **Interoperability:** Regardless of the language in which services are written, events can be persisted and propagated.
- **Real-time monitoring:** Systems can emit events as their states change, allowing an services or applications to search for anomalies and suspect activity.
- **Redundancy:** If a service is unavailable, events can be stored in the event broker until the service is ready to consume the event.
- **Microservices:** EDA is typically coupled with microservices to efficiently transport information between decoupled systems at scale.

## Advantages & Disadvantage of EDA
Event-driven architecture offers several advantages and have some disadvantages, including:

### Advantages
- **Asynchronous Communication:** Event-driven architectures are non-blocking. This allows each section to start the next task after completing its own task, without worrying about what happened before or after. They also cause events to be queued, which prevents event consumers from re-pushing their creators or blocking them.
- **Loose Coupling of Systems:** Services should not be dependent on each other. When using events, services operate independently without needing each other; including their implementation details and their protocol transmission. Services under the event-driven architecture can be updated, tested and deployed independently and more easily.
- **Scalability:** Because services under an event-driven architecture are isolated from each other, and because services usually perform only one task, it becomes easy to track down a specific service and navigate that service.
- **Resilience:** Due to the independence of the system components in the event-driven architecture, a failure in one service does not necessarily affect the entire system. In this isolated condition, repair and recovery from failure becomes easier.
- **Real-time processing:** Event-driven architecture is suitable for real-time processing of large amounts of data. With the occurrence of any event in the system, the operation can be executed in real time and the result can be obtained.

### Disadvantage
- **Complexity:** By separating too many dependencies, that work together easily, the architecture becomes harder and causes complexity in the design, service contracts or dependency graphs.
- **Inconsistencies:** Due to their asynchronous nature, event-driven models must carefully manage conflicting data between different services and versions. They usually do not support transaction ACID and instead support eventual consistency, which is more difficult to track or debug.

## Real examples of EDA
Event-driven architecture is used in a wide variety of applications, from e-commerce platforms to financial trading systems. Here are some real examples of event-driven architecture:
- **Uber:** Uber uses an event-driven architecture to power its real-time ride-hailing platform. Events such as taxi requests, driver availability and GPS data are used to match passengers with drivers in real time.

- **Netflix:** Netflix uses an event-driven architecture to power its recommendation engine. Events such as user viewing history, search history and user feedback are used to create personalized recommendations for each user.

- **Amazon:** Amazon uses an event-driven architecture to power its store platform. Events such as user search history, product visits, and purchase history are used to generate new offers and improve search results.

- **Airbnb:** The Airbnb platform uses an event-driven architecture to enhance its user experience. When a user searches for an accommodation, events are generated to identify available accommodation and filter results based on the user's search criteria. When a user books a property, events are generated to track the booking status, such as check-in and check-out times.

## Demo Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/EventDriven-Architecture-Demo">**Here**</a>, you can find the source code of very simple project and a demonstration of a event-driven software architecture on Github.

## Conclusion
Event-Driven Architevture (EDA) is an effective model for decreasing coupling between system components by modeling interactions with the notions of producers, consumers, events, and event-broker. An event indicates an action of interest and can be published and received asynchronously by components that are unaware of each other's existence. EDA enables components to operate and evolve independently. Where EDA is applicable, the advantages far outweigh the costs of implementation. It might be argued that EDA is a necessary component of every successful microservices deployment.