---
title: Clean Architecture
author: Amin Ziagham
date: 2022-12-28
categories: [Software Architecture]
tags: [C#, .NET, Clean Architecture, Software Architecture]
description: A clean architecture is a software architecture that each layer depends on and has access to the inner layer. Internal layers...
image: '/assets/img/posts/2022/12/CleanArchitecture-Open-Graph.jpg'
---

In this article, an attempt has been made to present the basic concepts and principles of clean architecture, along with a sample project. There are many proposed architectures for implementing software systems. Each of them has its advantages and disadvantages. Basically, a good architecture should be able to respond to the needs and changes of a software system in the long term. So that by changing the technology, changing the database, adapting to the new UI, etc., should not cause the loss of the structure, functionality, and complexity of the software code. Clean architecture is one of the few, and perhaps the best, architecture in this field that gives us the ability to reduce our perspective on dependencies and other long-term concerns.

## What is Clean architecture
A clean architecture is a software architecture that each layer depends on and has access to the inner layer. Internal layers do not have any references to higher layers. **Robert C. Martin**, known as **Uncle Bob**, tried to introduce a new architecture called Clean Architecture by examining and collecting the common characteristics of prominent and efficient architectures (e.g., Hexagonal Architecture, Onion Architecture, Lean Architecture, Multi-Layer Architecture, etc), which combines all the characteristics of those architectures in a single idea. Each of the mentioned architectures had unique characteristics and were somehow invented to solve and fix a problem. These important features include: Independent of Frameworks, Testable, Independent of UI, Independent of Database, Independent of any external agency, etc. Clean architecture tried to have all these important features together.

Figure 1, below, shows a simplified view of a clean architecture, which will be explained in the following.

{% include image.html url="/assets/img/posts/2022/12/CleanArchitecture.jpg" description="Clean Architecture diagram" title="Figure-1: Clean Architecture diagram" height="360px" width="360px" %}

The first and most important principle in this architecture is The Dependency Rule. In the image above, concentric circles show different areas of the software. In general, the more progress you make, the higher the level of the software. The outer circles are mechanisms and the inner circles are policies. The basic rule that makes this architecture work is The Dependency Rule. This rule says that source code dependencies can only be inward. Nothing in the inner circles can understand anything from the things in the outer circles and is unaware of them and their environment. If we move from the center of this architecture to the outside, the level of abstraction will decrease and we will enter more into the details of the implementation. In fact, moving inside the circle will lead to system policies, and moving outside this circle will bring us to the system mechanisms. In this architecture, we don't want anything in an outer layer to influence the inner part which includes the business rule.

Next, we will explain this architecture layer by layer.

## The Architecture's layers 
### Domain layer
This layer is considered as the heart of your application and responsible for your entities. Robert C. Martin defines Entity as:

<blockquote class="yellow">
Entities encapsulate Enterprise wide business rules
</blockquote>

Therefore, Entities can be a class or Object that has both methods and can only contain the structure of the entity itself. Enterprise in this sentence refers to large systems that are made up of different subsystems or applications, as a result, these entities can be used by different systems and therefore it does not matter how many different systems there are. They may use an entity. Entities are actually the same as Domain Entities in Domain Driven Design. If you do not have an Enterprise system and you write only one application, the entities are Business objects of this application. They include high-level and general rules. Entities are least likely to change when something outside changes. For example, you don't expect these objects to be affected by changes to page navigation or security. No operational changes in any particular application should affect the Entity layer. Entity validations, mapping data, using ORM, etc should not be placed in the domain layer. Our entities must be POCO and have only operations and properties related to our business. Other operations that are not related to entities should be transferred to external layers (like: the application layer).

> **Generally, these concepts are placed in domain layer:**<br/>
> Entities, Value Objects, Aggregates (if doing DDD), Enumerations

### Application layer
This layer contains business rules specific to your use case. These business rules are actually the use case and the rules of end-user interactions with the system. These use cases act as an orchestrator of data flow to or from entities. This layer should not have any effect on the status of entities. The work of this layer is simply coordinating the flow of data to or from entities based on business rules and use cases. External items such as UI or Database should not change the application layer. This layer is separated from such concerns. However, we expect changes in application performance to affect the use cases and therefore the software in this layer. If the details of a Use Case change, some of the code in this layer will certainly be affected.

> **Generally, these concepts are placed in application layer:**<br/>
> Application Interfaces, View Models / DTOs, Mappers, Commands / Queries (if doing CQRS), Logic, Validation, Application Exceptions

### Infrastructure layer
This is the outermost layer and it is where the most unstable components reside: network edges, operating systems, mailing services, devices, bus services, logging, etc. Most of the main details and concrete implementations of the interfaces in the application layer are done in this layer. As much as possible this layer and its implementations should be kept away from the stable domain layer.

> **Generally, these concepts are placed in application layer:**<br />
> Web services, Files, Message Bus, Logging, Configuration

### Persistence layer
This layer includes functions related to the database. Read, write, delete and search operations as well as database configuration are implemented in this layer. In other words, the type of database and how to interact with it are specified in this layer. In principle, the details of this layer can be integrated into the infrastructure layer and be a part of the infrastructure layer, but with increasing experience, we will come to the conclusion that this layer is better separated from the infrastructure layer and be a layer by itself (because it is possible to have different implementations of different databases).

> **Generally, these concepts are placed in application layer:**<br />
> Database configurations, Database driver, Repositories, ORM tools

### Presentation layer
This layer is the UI layer. As the name of this layer suggests, its task is to display data to the user. In essence, this layer is the entry point to the system from the user's point of view. In this layer, UI designs are placed. MVC/MVP and MVVM patterns are also placed in this layer. The important thing to mention is that this layer is absolutely not supposed to be a web application and return an HTML page. It could be a REST API instead. API Documentation (for the development environment) can also be used in this layer.

> **Generally, these concepts are placed in application layer:**<br />
> MVC Controllers, Web API Controllers, Open Api (e.g., Swagger / NSwag)

<blockquote class="yellow">
<b>NOTE:</b> In Clean Architecture, the number of layers is not necessarily fixed and limited to, for example, 4 or 5 layers. You can increase the number of layers in clean architecture depending on your needs and system type. What is important and must be observed is the law of dependency and maintaining its compliance. Moving towards the inner layers of this architecture, the level of abstraction increases and the outer layers contain more implementation details.
</blockquote>

## Advantages & Disadvantage of Clean Architecture
In this section we will list the advantages and disadvantages of Clean Architecture.

### Advantages
- **Protected logic:** Your business logic, in Application Layer, is protected, and nothing from the outside can make it fail. Your code does not depend on any external framework and it not controlled by someone else.
- **Flexibility:** In details and external layers, tools and frameworks can be replaced at anytime by any other implementation of your choiceand it can be done very quickly. It means that you can change the external components without affecting the core of the system.
- **Defer decisions:** What tool, what formwork, what database, etc. should we use? You can implement the logic of your application without considering the details and later decide to use the desired tools in higher layers.
- **Maintainability**: It’s easy to identify what component fails.
- **Easy to Develop:** As the architecture separates concerns, you can concentrate on one task at a time and develop faster. This also should reduce the amount of technical debt.
- **Tests:** Unit testing is easier as the dependencies are well-defined, it’s easy to mock or stub.

### Disadvantage
- **Learning curve:** At the beginning, the architecture can be overwhelming, especially for junior developers.
- **Multitude of projects:** More classes, more packages, more sub-projects. To my knowledge, there is nothing much that can be done about that.
- **Complexity:** The complexity of the project is higher and it is not recommended for small projects.

## Demo Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/CleanArchitecture-Demo">**Here**</a>, you can find the source code of very simple project and a demonstration of a clean architecture on Github.

## Conclusion
The clean architecture was introduced in this article. Clean architecture is a model that each layer depends on and has access to the inner layer. On the other hand, internal layers do not have any references to higher layers. Compared to other software architectures (e.g., Multi-Layer Architecture), Clean Architecture is a completely testable and more maintainable solution that can adapt to changes faster. Naturally, this architecture is not a Silver Bullet, and in addition to its many tangible and intangible benefits, it will also have problems and difficulties. The goal here was just an initial introduction to this architectural style. A demo project has also been implemented and linked in this article, which can be used to learn more concretely about the topics discussed. There are many more details of the parts and rules and implementation of this architecture in the <a target="_blank" href="https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164">**Clean Architecture book**</a>; You can refer to this valuable book.