---
title: The Command Query Separation (CQS) Design Principle
author: Amin Ziagham
date: 2023-01-29
categories: [Design Patterns]
tags: [C#, .NET, WebApi, Mediator pattern, MediatR, Design patterns, Command, Query, CSQ, CQRS, Postman, Swagger, REST Client]
---

This article aims to describe the Command Query Responsibility Separation (CQRS) design principle, its advantages and disadvantages, and how to implementing this pattern in .NET with an example. First, the basic explanations and concepts will be mentioned, then the given explanations will be implemented with an example.

## Command and Query definition
### Command
In essence, **Command** represents the requests to create or change the state of an Entity. They change an entity through operations like Insert, Update, and Delete. Commands are an object whose name represents the name of an operation. This object usually requests the application or server to perform an operation. The data required to perform that operation are also inside the command. Generally, these objects are named with Command text at the end of their names. For example, below shows a command to add a product:

<script src="https://gist.github.com/ziagham/d792f95d3c2f6d4f997fcdaecf4b5359.js"></script>

### Query
On the other hand, **Query** is a request to receive or retrieve information about an Entity. In simple words, any request that changes the data in the system is called a Command, and any request that does not change the data and only displays it is called a Query. Queries are objects that only have the desired methods to receive information. Query objects return **Data Transfer Objects (DTOs)** as a result that can be used in the UI or elsewhere. Their names usually start with Get. For example, below shows a query to get products list:

<script src="https://gist.github.com/ziagham/7e46af616f34d0461e5159e2fae7cb01.js"></script>

## Definition of CQS
Before we go to CQRS, it is better to take a look at CQS. CQS stands for Command Query Separation and is a code pattern that creates this capability to separate requests from commands. So that requests are in a separate flow from commands. This code pattern will not bring more code for us, and on the database side, no special difference will be applied, and in practice, both requests and commands will have a common bounded context. The **Figure-1** provides a very simple diagram of CQS. As you can see, requests are separate from commands and the respective services will process them. But the database, or any other information storage source, is shared.

{% include image.html url="/assets/img/posts/2023/01/CQS.jpg" description="A simple diagram that shows the most important aspects of the CQS" title="Figure-1: A simple diagram that shows the most important aspects of the CQS" %}

## Definition of CQRS
CQRS stands for Command Query Responsibility Segregation and is an architectural pattern. In CQRS, as in CQS, both request and command services are separated, but in addition, we have another separation at the database level. So that bounded contexts are completely separate for requests and commands (or Reads and Writes). Figure-2 shows a simple diagram of the CQRS architecture. As can be seen, in addition to decoupling at the code level, decoupling has also occurred at the database level.

{% include image.html url="/assets/img/posts/2023/01/CQRS.jpg" description="A simple diagram that shows the most important aspects of the CQRS" title="Figure-2: A simple diagram that shows the most important aspects of the CQRS" %}

One of the important features of CQRS is that the optimization methods for each database can be considered separately. Because the Read database is separate from the Write database, optimization methods and performance enhancement methods can be implemented separately without worrying about the negative impact on each other. For example, on the read database side, indexing can be used to increase reading speed, without worrying about decreasing writing speed. Because basically, writing is not done in this database. On the other hand, for the Write database, techniques can be considered to increase the writing speed, without worrying about the impact on the read database. But a very important point to note is that the read and write database must be Sync. Of course, this should happen in practice. In another article, I will explain the tradeoff between syncing in CQRS and **CAP Theorem**.

<blockquote class="yellow">
<b>NOTE:</b> CQRS will have better performance due to the separation of read and write operations and the subsequent application of optimization techniques. However, CQS, which is a code-level separation, will not improve the performance of the application, and we will only have a cleaner solution.
</blockquote>

## Advantages & Disadvantage of CQRS
Before we go to the implementation part, it is better to list the advantages and disadvantages of using CQRS and CQS.

### Advantages
- **Seperation Concerns:** In both CQRS and CQS, by separating requests and commands, it somehow helps manage complexity and makes the program more maintainable and extensible.
- **Optimization:** Since in CQRS, read and write databases are separate from each other, optimization methods can be applied separately for each database to improve performance.
- **Scalability:** Since the workloads between reading and writing operations are separated, it makes the application scalable and easy to develop. In the future, if it is necessary to make arrangements to query the database, these changes can be easily made at the code level.
- **Single responsibility principle:** Both CQRS and CQS allow us to have a separate model for each command and query and develop each one separately. Therefore, CQRS follows Single Responsibility principle (SRP) at the project architecture level, where each model has only one task apart from other models.

### Disadvantage
- **Complexity of Code:** It may increase the complexity of the code and program. In some parts of the program, writing needs to happen after or before reading, in which case it may cause complexity in CQRS principle. In reality, in large systems, we need to update data after reading. For example, a user login to system (read) and we want to store information about the user like location, IP, atc. (write).
- **Synchronization:** As mentioned, in CQRS, the Read and Write databases are separate from each other. Therefore, these databases must be consistent with the others. Thus, there may be a delay in the database's synchronization.
- **Not applicable in simple projects:** CQRS, and likely CQS, will present some complexity (e.g., Synchronization delay) to the system. Therefore, it is not recommended that simple applications that only perform basic CRUD operations use CQRS.

## Demo Project Link
In this section, a simple project has been implemented about the CQS pattern. It should be noted that this project only includes CQS and does not include the CQRS pattern. In the future and after the introduction of some other technologies, the CQRS algorithm will also be implemented.
<a target="_blank" href="https://github.com/NextCodeBlock/CQS-Demo">**Here**</a>, you can find the source code of very simple project and a demonstration of a the CQS concept on Github.

<blockquote class="yellow">
<b>NOTE:</b> In both CQRS and CQS, using the Mediator pattern brings many advantages. Every command and query in the system is connected to its respective Handler using Mediator pattern, and the response to the request is also provided through this Handler. The Mediator pattern causes loose coupling. Therefore, the MediatR library has been used for this purpose in the demo project.
</blockquote>

## Conclusion
The CQRS and CQS design principles, and their adventages and disdvantages, were introduced in this article. Both principles allow us this capability to separate requests from commands. This capability have some advantages like it can perform optimization on CRUD operations, scale the application, stc. These patterns are not recommended for small and simple projects. Because it will increase the complexity and understanding of the program and may create computational overhead. But it is strongly recommended for large projects with a lot of reads and write operations from/to the database. In the end, a demo project has been implemented, which tries to implement the mentioned concepts.