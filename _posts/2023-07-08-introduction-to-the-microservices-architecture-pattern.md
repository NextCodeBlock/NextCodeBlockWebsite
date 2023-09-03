---
title: Introduction to the Microservices Architecture Pattern
author: Amin Ziagham
date: 2023-07-08
last_modified_at: 2023-07-08
categories: [Software Architecture]
tags: [C#, .NET, Monolithic, Microservices, SOA, Service Oriented Architecture, Software Architecture]
description: Microservice architecture are a collection of small, discrete services that work together to form a larger, more complex application...
image: '/assets/img/posts/2023/07/Microservices-OG.jpg'
---

With the increasing demand for large enterprise applications, a concept known as microservices has expanded throughout the software development industry, as developers are forced to adopt microservices architecture to construct large business systems. However, the concept of microservices appears to be a bit hard and complicated, but this is never the case, and in this post we will look at what, why, and the advantages and disadvantages of using microservices. But, before we go into the intricacies and grasp the nature of microservices architecture, it is important to understand why and how the process leading to microservices design evolved.

## What is Monolithic architecture
Monolithic architecture, which is also called integrated architecture, is the traditional model of application design. In Monolithic architecture we are dealing with an integrated codebase where all functions and capabilities of the application are placed together and interdependent. In a monolithic architecture, all code is highly interdependent and difficult to modify. However, monolithic architecture has been the main method of software development for years. The complexity and difficulty of monolithic software development increases with the addition of new parts to the software.

Monolith architecture has taken numerous shapes throughout the years, including single blocks of software such as desktop programs and distributed systems such as 2-Tier, 3-Tier, and <a href="/posts/multi-layer-software-architecture/">**N-Tier**</a>. These are all instances of Monolithic Architecture.

{% include image.html url="/assets/img/posts/2023/07/Monolithic-Architecture-Pattern.jpg" description="Monolithic Architecture Pattern" title="Figure-1: Monolithic Architecture Pattern" %}

### Monolithic architecture's disadvantages
However, the monolithic architecture have many disadvantages that prevent it from being used for large business systems. Some of its most important disadvantages are stated below:

- **Development:** The development rate of this type of system is slow. A system with multiple services, each with many unit tests, may take a long time (e.g. 1 hour) to complete (assuming the CI/CD pipeline does not fail for some reason). On the other hand, with such systems, all developers often use the same technological stack.
- **Coupling:** Because these systems frequently rely on the same "server function," their services or components are typically integrated and tightly coupled. Even if these services or components are implemented utilizing the SOLID and OOP paradigms, they may be more tightly connected because they are packaged in the same binary.
- **Scalability:** It can be difficult to scale monolithic architectural software since it is tightly connected. You must drag the entire architecture up with you as your codebase develops and/or new features are added. Even if you only wish to improve or change one function, the complete application must be modified. This not only wastes time and resources, but it also has the potential to disrupt your continuous delivery.
- **Performance:** Performance in monolithic systems must be planned for the entire application. This include Performance testing. Parts of the system under heavy strain may cause the entire system to slow down. 

{% include advertisements.html %}

## What is Microservices Architecture
Microservice architecture, in contrast to monolithic architecture which considers the application as a **"single whole"**, are a collection of small, discrete services that work together to form a larger, more complex application. This architecture allows for increased scalability and flexibility, as each service can be updated or modified without affecting the entire application. In addition, each services has its own logic, codebase, and database. Keep in mind that in this definition, when we talk about "smaller parts", we mean independent modules that are deployed individually and do not need each other, but interact with each other through API to form a larger program. Furthermore, microservices are often designed to be language-independent, meaning that different services can be written in different programming languages, providing more flexibility to the development process. 

Microservice is a type of **SOA (service Oriented Architecture)** that has been very popular in the past decades, but at the same time, microservice is more flexible than service-based architecture because it is easy to take a service or module from a project and use it without special configuration. It was used in another project, but the SOA architecture is implemented inside a so-called Monolithic architecture.

The figure 2, below, illustrates a simple view of a microservices architecture.

{% include image.html url="/assets/img/posts/2023/07/Microservices-Architecture-Pattern.jpg" description="Microservices Architecture Pattern" title="Figure-2: Microservices Architecture Pattern" %}

> The important thing to say is that different services in a microservice architecture will communicate with each other using HTTP requests and RESTful APIs.

As can be seen, microservice architecture consists of relatively many components (e.g., Identity provider, Api gateway, service discovery, JWT, MessageBroker, etc). It will be tried to describe each of these components in separate posts.

### Characteristics of Microservices
If we want to summarize the characteristics of microservices, it will be as follows:
- Each service has its own database layer, codebase, and set of CI/CD tooling.
- It is the responsibility of services to maintain their data or external state.
- Every service can be deployed individually and tested without relying on other services.
- Internal communication between services is accomplished through the use of well-defined APIs or any lightweight communication mechanism.
- Each service can choose the technological stack, libraries, and frameworks that are most suited to its use cases.
- If a network or system failure occurs, services should implement Retry functionality.

### Advantages & Disadvantage of microservices
#### Advantages
- **Independent components:** All services of this architecture can be updated and deployed separately. This independence means that if there is a bug in a specific service, the whole applications will not fail, but the specific service will suffer. Finally, adding new features to the applications is much easier than a monolith architecture.
- **Easier understanding:** Naturally, applicationss with microservices architecture do not have a single source code, but there is separate source code for each of the desired services, and these source codes are not directly dependent on each other. This makes the code much easier to read and understand.
- **Scalability:** One of the important advantages of the microservices architecture is that their scalability is infinite because each of the services can be upgrated individually. This issue saves our financial resources because in monolithic architecture we have to upgrade the whole application even if we don't need it, but in microservices we can upgrade only a specific part of the application. Of course, it should be noted that a server can be upgraded to some extent and eventually we hit a wall, but microservices do not have walls.
- **Faster deployments:** Smaller codebases and scope mean faster deployments, allowing you to experiment with Continuous Deployment.
- **Flexibility in choosing technology:** Since the number of microservices is very large and all kinds of services are available for you, there is no limit in choosing technologies. Also, since most interactions of the applications are through API, you can use any technology and package to implement your applications.
- **Fault Tolerance:** Since each component of the applications is independent, the creation of bugs and errors in one of them does not paralyze other parts of the program.

#### Disadvantages
- **Complexity:** Since applications with microservices architecture are distributed components, you need to establish connections between individual components and consider your database as well. Also, deploying the components is done separately, so you have to do several deploy operations to have a functionable application.
- **System distribution:** System distribution in microservices is done manually and requires a lot of attention.
- **System integration:** When you implement an application based on microservices, its integration becomes a bit difficult because all its parts are separate. For example, the log system, the metrics system (monitoring the application's performance and its response speed), running health tests on the application, and the like require coordination between all application components.
- **Testing:** Writing tests for applications that constantly use different APIs is a much more difficult task.

### Microservices Architecture use cases
Because of the various advantages of the microservices architecture, it is frequently used to design software solutions in a variety of industries. The following are the top four microservices use cases:
- **Big data applications:** Applications that must process a large amount of data obtained from many sites typically have a large number of dependents. Since, microservices coupled with <a href="/posts/eventdriven-architecture-pattern/">**Event-driven Architecture (EDA)**</a>, then they are a natural fit for complex applications. Microservices aid in the efficient use of resources, lowering the complexity of an application's architecture.
- **Real-time data processing:** The amount of data that must be processed and stored in real time may vary greatly. Microservice architecture-based applications can assign the necessary amount of data storage and computing power to specified services on demand. Indeed, in microservices architecture, the publish-subscribe messaging pattern enables smooth, asynchronous communication to process and analyze real-time data for streaming platforms to generate intelligent outputs.
- **Multi-group developments:** To achieve frequent release deadlines, the software development space is frequently made up of numerous developers working on the same functionality of an application. Microservices architectures enable applications to be separated into discrete services that may be controlled and plugged in by individual groups, reducing possibilities of code surrendering on "merge day.
- **Highly-Resilient Applications:** Apps designed for areas such as healthcare or finance should be 100% uptime and securely store data. Developers can design extremely resilient and secure apps with the help of the microservice architecture. Even if one or more microservices fail, the entire application can continue to run, allowing access to the remaining functionality.

### Agility and speed of delivery ratio
Due to the fact that applications designed with microservices have a higher deployment speed, compared to monolithic applications, therefore, they can adopted themself with the scalibility and agility. The graph below, Figure-3, compares monolithic-based development and microservices to modern enterprise challenges such as agility, speed of delivery, and scale.

{% include image.html url="/assets/img/posts/2023/07/Agility-Speed-Of-Delivery.jpg" description="Agility and speed of delivery ratio for Microservices verses Monolithic" title="Figure-3: Agility and speed of delivery ratio for Microservices verses Monolithic" %}

> In comparison to typical monolithic applications, microservices promise more agility, speed of delivery, and scale.

### Development and deployment cost and time ratio
Businesses no longer invest in massive application development projects with the turnaround time of a few years. Unlike a few years ago, enterprises are no longer interested in designing integrated and consolidated applications to manage their end-to-end business processes. The graph below, Figure-4, compares the state of traditional monolithic applications vs microservices in terms of turnaround time and cost.

{% include image.html url="/assets/img/posts/2023/07/Cost-Turn-Around-Time.jpg" description="Development and deployment cost and time ratio for Microservices verses Monolithic" title="Figure-4: Development and deployment cost and time ratio for Microservices verses Monolithic" %}

> Microservices provide an approach for developing quick and agile applications, resulting in less overall cost.

## Conclusion
This article discussed the nature of microservices, the differences with monolithic, and the process that led to the emergence of microservices. An overview of their advantages and disadvantages, characteristics, and use cases was also presented. Also, a brief comparison of microservice-based and monolithic-based applications was presented in terms of Agility, speed of delivery, scale, development cost, and deployment cost. Along with all the advantages and disadvantages of microservices, it should be noted that this architecture is mostly used to design and implement complex commercial systems.