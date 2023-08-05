---
title: Asynchronous messaging in .NET using RabbitMQ
author: Amin Ziagham
date: 2023-01-18
last_modified_at: 2023-01-18
categories: [Design Patterns]
tags: [C#, .NET, WebApi, Asynchronous messaging, RabbitMQ, Postman, Swagger, REST Client]
---

This article aims to describe the Command QueryResponsibility Separation (CQRS) design principle, its advantages and disadvantages, and how to implementing this pattern in .NET with an example. First, the basic explanations and concepts will be mentioned, then the given explanations will be implemented with an example.

## Command and Query definition
### Command
In essence, **Command** represents the requests to create or change the state of an Entity. They change an entity through operations like Insert, Update, and Delete. Commands are an object whose name represents the name of an operation. This object usually requests the application or server to perform an operation. The data required to perform that operation are also inside the command. Generally, these objects are named with Command text at the end of their names. For example, below shows a command to add a product:

<script src="https://gist.github.com/ziagham/d792f95d3c2f6d4f997fcdaecf4b5359.js"></script>

### Query
On the other hand, **Query** is a request to receive or retrieve information about an Entity. In simple words, any request that changes the data in the system is called a Command, and any request that does not change the data and only displays it is called a Query. Queries are objects that only have the desired methods to receive information. Query objects return **Data Transfer Objects (DTOs)** as a result that can be used in the UI or elsewhere. Their names usually start with Get. For example, below shows a query to get products list:

<script src="https://gist.github.com/ziagham/7e46af616f34d0461e5159e2fae7cb01.js"></script>

## Definition of CQS
Before we go to CQRS, it is better to take a look at CQS. CQS stands for Command Query Separation and is a code pattern that creates this capability to separate requests from commands. So that requests are in a separate flow from commands. This code pattern will not bring more code for us, and on the database side, no special difference will be applied, and in practice, both requests and commands will have a common bounded context. The **Figure-1** provides a very simple diagram of CQS. As you can see, requests are separate from commands and the respective services will process them. But the database, or any other information storage source, is shared.

{% include image.html url="/assets/img/posts/2022/11/CQS.jpg" description="A simple diagram that shows the most important aspects of the CQS" title="Figure-1: A simple diagram that shows the most important aspects of the CQS" %}

## Definition of CQRS
CQRS stands for Command Query Responsibility Segregation and is an architectural pattern. In CQRS, as in CQS, both request and command services are separated, but in addition, we have another separation at the database level. So that bounded contexts are completely separate for requests and commands (or Reads and Writes). Figure-2 shows a simple diagram of the CQRS architecture. As can be seen, in addition to decoupling at the code level, decoupling has also occurred at the database level.

{% include image.html url="/assets/img/posts/2022/11/CQRS.jpg" description="A simple diagram that shows the most important aspects of the CQRS" title="Figure-2: A simple diagram that shows the most important aspects of the CQRS" %}

One of the important features of CQRS is that the optimization methods for each database can be considered separately. Because the Read database is separate from the Write database, optimization methods and performance enhancement methods can be implemented separately without worrying about the negative impact on each other. For example, on the read database side, indexing can be used to increase reading speed, without worrying about decreasing writing speed. Because basically, writing is not done in this database. On the other hand, for the Write database, techniques can be considered to increase the writing speed, without worrying about the impact on the read database. But a very important point to note is that the read and write database must be Sync. Of course, this should happen in practice. In another article, I will explain the tradeoff between syncing in CQRS and **CAP Theorem**.

<blockquote class="yellow">
<b>NOTE:</b> CQRS will have better performance due to the separation of read and write operations and the subsequent application of optimization techniques. However, CQS, which is a code-level separation, will not improve the performance of the application, and we will only have a cleaner solution.
</blockquote>

## Advantages & Disadvantage of CQRS
Before we go to the implementation part, it is better to list the advantages and disadvantages of using CQRS.

### Advantages
- **Seperation Concerns:** In CQRS, by separating requests and commands, it somehow helps manage complexity and makes the program more maintainable and extensible.
- **Optimization:** Since in CQRS, read and write databases are separate from each other, optimization methods can be applied separately for each database to improve performance.
- **Scalability:** Since the workloads between reading and writing operations are separated, it makes the application scalable and easy to develop. In the future, if it is necessary to make arrangements to query the database, these changes can be easily made at the code level.
- **Single responsibility principle:** The CQRS allows us to have a separate model for each command and query and develop each one separately. Therefore, CQRS follows Single Responsibility principle (SRP) at the project architecture level, where each model has only one task apart from other models.

### Disadvantage
- **Complexity of Code:** It may increase the complexity of the code and program. In some parts of the program, writing needs to happen after or before reading, in which case it may cause complexity in CQRS principle. In reality, in large systems, we need to update data after reading. For example, a user login to system (read) and we want to store information about the user like location, IP, atc. (write).
- **Synchronization:** As mentioned, in CQRS, the Read and Write databases are separate from each other. Therefore, these databases must be consistent with the others. Thus, there may be a delay in the database's synchronization.
- **Not applicable in simple projects:** CQRS will present some complexity (e.g., Synchronization delay) to the system. Therefore, it is not recommended that simple applications that only perform basic CRUD operations use CQRS.

## Sample Implementation
In this part, a very demonstartion program will be writen to show you how to use MediatR library to implemented mediator pattern in .Net application. Our example is a web API example that lists products and also finds a product based on its id. In this example, it has been tried to implement GetAllProducts and GetProductById operations separately and in two different handlers. Then mediatR will map the requests to the corresponding handlers for us. This project implemented via .NET 6.0 and C# programming language. You can use VSCode, Visual Studio, or any other IDE or text editor. 

### MediatR Library
The MediatR library is an open source project that is implementation of the Mediator pattern in .NET that supports Request and Command. In MediatR, each request is placed in a queue in the memory and then connected to the Handler corresponding to that request. MediatR has no dependencies on other libraries and uses generics in C# to create Response and also to determine Handlers. Another very good feature of MediatR is the support of the Pub/Sub template, by which you can create a notification and call this notification in different places of the project.

<a target="_blank" href="https://github.com/jbogard/MediatR">**Here**</a> you can see the link to the MediatR project on github.

<blockquote class="yellow"><b>NOTE:</b> In this article, we don't intend to use the Pub/Sub feature and we will focus mostly on Request and Command.</blockquote>


First, create a web API project. You can do this using Visual Studio or use .NET CLI for it. Then, install the <a target="_blank" href="https://www.nuget.org/packages/MediatR">**MedaitR**</a> and <a target="_blank" href="https://www.nuget.org/packages/MediatR.Extensions.Microsoft.DependencyInjection">**MediatR.Extensions.Microsoft.DependencyInjection**</a> libraries. You can do this by using .NET cli with the following commands.

```console
dotnet add package MediatR
dotnet add package MediatR.Extensions.Microsoft.DependencyInjection
```

or doing this through Visual Studio Package installer by following commands:

```console
Install-Package MediatR
Install-Package MediatR.Extensions.Microsoft.DependencyInjection
```

After installing the desired package, create two folders called **Queries** and **Services** in your project. The structure of your solution should look something like the figure below.

{% include image.html url="/assets/img/posts/2022/11/MediatR_Demo_Implementation_SolutionExplorer.jpg" description="Solution structure" title="Figure-3: Solution structure" %}

In the Services folder, we create a class called Product and write the following code in it.
<script src="https://gist.github.com/ziagham/508a15b70383e2e9d5a98781886bc414.js"></script>

The next step is to write a service for the product that has the task of listing products as well aso geting details of product by its Id. For this, we create the interface **IProductService** and **ProductService** and write the following codes in them.

<script src="https://gist.github.com/ziagham/4755d460a56ad31b3a44216c905b4926.js"></script>

<script src="https://gist.github.com/ziagham/87455b81e5cecc39741061e4f36d599a.js"></script>

The next step we need to do is create the Request and the corresponding Handler for every requests or commands we want to do. In practice, we only call the request. It is MediatR, or any other mediator pattern library, that is responsible for finding and mapping the request with its corresponding handler. Inside the Queries folder, which was created, we add two classes named **GetAllProductsQuery** and **GetProductByIdQuery**. Like the Figure-4 as below.

{% include image.html url="/assets/img/posts/2022/11/MediatR_Demo_Implementation_SolutionExplorer_With_Queries_Expand.jpg" description="Solution structure with Queries expand folders" title="Figure-4: Solution structure with Queries expand folders" %}

GetAllProductsQuery is responsible for returning the list of products and GetProductByIdQuery is responsible for finding the product based on the desired ID. The following codes are related to these two classes.

<script src="https://gist.github.com/ziagham/010ba38505ccd8e6ffbbe3deeaeb1218.js"></script>

<script src="https://gist.github.com/ziagham/900afcc250c78b1ece648de14a34f634.js"></script>

The next step we need to do is to create an instance of IMediator and inject it into the api controller's constructor. After that, the created requests can be called in each of the methods and actions in the controller. The following code demonstrates this.

<script src="https://gist.github.com/ziagham/208f3313de319196f8d0c65b756033d9.js"></script>

At the end, it is time to register ProductService and MediatR. We add them in ConfigureServices (inside startup.cs or it may be minimal api) as below.

<script src="https://gist.github.com/ziagham/a590b27b3add7edee872a1bd4032c976.js"></script>

No you can compile and run it. After running, you can test the Api and check the correctness of its operation.

<blockquote class="yellow"><b>NOTE:</b> There are many tools for API testing. The most famous of them is <a target="_blank" href="https://www.postman.com"><b>Postman</b></a>, which is used by many backend developers. If you are in the development environment, you can use openApi tools such as <a target="_blank" href="https://www.nuget.org/packages/swashbuckle.aspnetcore/"><b>Swagger</b></a> for .NET, which in addition to API documentation, you can also run your API. If you are using an IDE like VSCode, the <a target="_blank" href="https://marketplace.visualstudio.com/items?itemName=humao.rest-client"><b>REST Client</b></a> extension can perform this operation for you. Just note that the REST Client is a scripting environment and does not have a GUI.</blockquote>

For example, as can be seen in the image below, the Swagger tool used to facilitate works with REST-API.

{% include image.html url="/assets/img/posts/2022/11/Swagger_OpenApi_Execution_Product_Api_With_MediatR.jpg" description="Swagger OpenApi Execution Product Api With MediatR" title="Figure-5: Swagger OpenApi Execution Product Api With MediatR" %}

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/MediatR-Demo">**Here**</a>, you can find the source code of this project on Github.

## Conclusion
The mediator pattern is one of the patterns that reduce the complexity and dependency between components. In this article, this pattern (mediator pattern) and its advantages and disadvantages were investigated. In the following, the MediatR library was introduced and it was shown how implementing the Mediator pattern using the MediatR library is a very easy and hassle-free task, which, in addition to providing a clean solution, helps a lot to create loose coupling between different components. The given example was implemented within an ASP.NET WebApi project.