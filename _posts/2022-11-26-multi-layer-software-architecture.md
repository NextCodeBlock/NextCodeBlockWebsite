---
title: Multi-Layer Software Architecture
author: Amin Ziagham
date: 2022-11-26
categories: [Software Architecture]
tags: [C#, .NET, Multi-layer, N-Tier, Software Architecture]
description: Multi-layer architecture is one of the most widely used and proven model for software architectures. For several decades...
---

One of the most popular architectural patterns for software development is **Multi-layered software architecture**. By using this architecture, you can speed up the process of doing developing and also you can control and balance the complexity of applications that are increasing every day.

There is also another pattern called **N-Tier architecture**, which is different from Multi-layer architecture. Many developers and software engineers consider these two patterns are the same. This is while conceptually and practically, these two are different. In the following, the difference between the two will be mentioned. But the main focus of this article will be on Multi-layer software architecture.

## What is N-Tier architecture
An N-Tier Application is a program distributed among three or more separate machines or computers in a distributed network. The word Tier means category, class, or level. The most common form of application or n-tier architecture is the three-tier architecture, which is classified into three tier:

- **Presentation**: Execute and run on the user's computer or device.
- **Application**: Hosted in a centralized computer or server and responsible to service to the UI.
- **Database**: The database that is needed is located on a separate computer. This separate computer is restricted by various rules and in most cases only the DBA is allowed to access and manage it. The business logic layer also connects to this computer database with different methods (such as connection string) and receives service.

The figure 1, below, shows a view of a three-tier architecture.

{% include image.html url="/assets/img/posts/2022/11/Three-Tier-Architecture.jpg" description="A simple diagram that shows the three-tier architecture" title="Figure-1: A simple diagram that shows the three-tier architecture" %}

As mentioned, 3-Tier is the basic and the most common form of n-tier architecture, but these tiers can be variable and change according to system requirements. For example, in the figure 2, with the increase in the number of UIs and to integrate the URLs consumed by UIs aswell as load balancing, a fourth layer (Gateway) has been added, which is responsible for directing UI requests to business logic.

{% include image.html url="/assets/img/posts/2022/11/Four-Tier-Architecture.jpg" description="A simple diagram that shows the four-tier architecture" title="Figure-2: A simple diagram that shows the four-tier architecture" %}

## What is Multi-Layer software architecture
Multi-layer architecture is one of the most widely used and proven model for software architectures. For several decades, this architecture was the main choice of architectures and software engineers for the production and development of software systems. This architecture is still effective for designing small and medium-sized software systems, but with the increasing complexity and scalability of software systems, other architectures were introduced and used.

Unlike N-Tier, in Multi-layer software architecture, the entire application is divided into several logical parts and these parts are not physically separated and run on the same computer. 

<blockquote class="yellow">
<b>NOTE:</b> Difference between tier and layer: A "layer" refers to a functional division of software, but a "tier" refers to a functional division of software running on a separate infrastructure from other parts. For example, the contacts application on a mobile phone is a three-layer software architecture but single-tier application, because all three layers run on the same mobile device. This is an important difference because layers cannot provide the same facilities as tier.
</blockquote>

The most common and simplest mode of multi-layer software architecture is 3 layers. These three layers are:
- **Presentation Layer**: The presentation layer is the highest and first layer that is displayed in applications. This layer receives the information related to the commonly available services and displays them in a web browser, web application, mobile app, desktop application, etc. in the form of a graphical user interface (GUI).
- **Business Logic Layer (BLL)**: This layer also called the "middle layer", "business logic layer" or "logic layer", is the heart of an application (software). In this layer, the information received from the display layer is processed. The application layer can also edit or delete the data in the data layer or add new data to the data layer.
- **Data Access Layer (DAL)**: The data access layer is where the data processed by the business logic layer is prepared and managed in order to store in a database. This layer is responsible for accessing the database. For this purpose, entities, ORM (Object–relational mapping), repositories, etc. are located and implemented in this layer.

In the figure 3, you can see a simple diagram of the three-layer software architecture.

{% include image.html url="/assets/img/posts/2022/11/Three-Layer-Architecture.jpg" description="A simple diagram that shows the three-layer software architecture" title="Figure-3: A simple diagram that shows the three-layer software architecture" %}

The number of layers in multi-layered software architecture can be increased according to conditions and requirements. For example, in a software system, common models between all layers can be logically separated and placed inside another layer. Also, this theorem can be implemented for models and classes of settings and configurations, and a separate layer was considered for them. Figure 4 illustarte the Five-layer softawre architecture which two layers, common models and configuration layer, have been added.

{% include image.html url="/assets/img/posts/2022/11/Five-Layer-Architecture.jpg" description="A simple diagram that shows the five-layer software architecture" title="Figure-4: A simple diagram that shows the five-layer software architecture" %}

## Advantages & Disadvantage of Multi-Layer Architecture
Before we go to the implementation part, it is better to list the advantages and disadvantages of using CQRS.

### Advantages
- **Easy to Learn:** Its concepts are simple and it is easier to learn and implement than other advanced architectures.
- **Reduced Dependency:** Due to the seperation of the functionality of each layer from the other layers, dependencies are reduced.
- **Testing:** Because each layer is separate, it will be easier to test.
- **Low Cost Overheads:** The development and maintenance cost of this software development model is lower than advanced architectures.

### Disadvantage
- **Scalability:** Due to the layered nature of the architecture and the fact that the layers are dependent on the underlying layer, it is very difficult and complicated to scale this architectural model.
- **maintainability:** Due to the tight coupling between layers, a change in one layer can affect the performance and functionality of the entire system. Therefore, it is difficult to maintain this software architecture.

## Demo Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/MultiLayerArchitecture-Demo">**Here**</a>, you can find the source code of very simple project and a demonstration of a three-layer software architecture on Github.

## Conclusion
Multilayer software architecture was introduced in this article. Also, its difference with Multi-tier architecture was also stated. In addition, it's mentioned that the most common and simplest mode of multi-layer software architecture is three-layers which include the presentation layer, business layer, and data access layer. Learning this architecture is very simple. Also, the dependency within the system is low and its testing is simple. But on the other hand, the scalability and maintainability of this software architecture is hard and complicated.