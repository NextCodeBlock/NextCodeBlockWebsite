---
title: Facilitating Mediator Pattern in .NET by using MediatR
author: Amin Ziagham
date: 2022-11-13
categories: [Design Patterns]
tags: [C#, .NET, WebApi, Mediator, MediatR, Design patterns, Postman, Swagger, REST Client]
---

This article aims to describe the mediator design pattern and how to facilitating using this pattern in .NET with an example. First, the basic explanations and concepts will be mentioned, then the given explanations will be implemented with an example.

## Mediator pattern
The Mediator pattern is one of the behavioral patterns of Gang of Four (GOF) Design Patterns. The use of this design pattern causes integration and reduction of disorder in the dependency between objects. In this pattern, any direct communication between two objects will be limitted and they force to use an interface object called Mediator to communicate with each other.

Without using the mediator pattern, the complexity of communication between different components can be very high. For example, in the Figure-1, four components must be connected to each other in order to sync, update, or communicate (for any purpose). As can be seen, for four components, the complexity is high. Now imagine increasing the number of these components! What a mess and complexity will arise. The result of these complications leads to the implementation becoming more difficult and increasing the communication overhead.

{% include image.html url="/assets/img/posts/2022/11/Without_Mediator_Pattern_Diagram.jpg" description="Communication between different components without Mediator pattern" title="Figure-1: Communication between different components without Mediator pattern" %}

On the other hand, when we use the mediator pattern, the dependencies and connections become loose and it gives us the ability to apply changes more easily. In the figure below, you can see a schematic view of the using mediator pattern. 

{% include image.html url="/assets/img/posts/2022/11/Mediator_Pattern_Diagram.jpg" description="Communication between different components with Mediator pattern" title="Figure-2: Communication between different components with Mediator pattern" %}

Similar to all design patterns, this pattern also has its advantages and disadvantages, which will be discussed further.

### Advantages
- **Loose coupling:** It causes the reduction of coupling in the code, which reduces the repetition of the code as a result.
- **Reusability:** Each of the required components can be replaced with other new components without affect other part of the pattern.
- **Less communication:** If a component wants to communicate with another component, it just needs to send a command to mediator.
- **Follow the SRP:** Mediator pattern follows the Single Responsibility principle, from the SOLID principles. Due to the reduction of communication between components, the main logic will be placed inside the mediator. Because of this, the components don't contain logic, which means they only have one reason to change their class.
- **Follow the OCP:** Mediator pattern follows the Open/Closed principle, from the SOLID principles. We can simply add another Mediator class in our code without making any changes to the components.

### Disadvantage
- **Intimate Mediator Behavior:** Because the mediator contains the main logic, it will grow over time and become a crucial factor in your application. This growth might lead to a "God object".
- **Maintability:** Because all communication among components is through the mediator, and on the other hand mediator object is a central point to control interactions between components, naturally maintaining an object like the mediator will be a challenge.

<blockquote class="yellow">
<b>NOTE:</b> In this article, we do not intend to implement the mediator pattern. Our intention is to use a tool to facilitate the use of this pattern in .NET. Therefore, you can refer to the <a target="_blank" href="https://refactoring.guru/design-patterns/mediator"><b>Here</b></a> for additional information. There, in addition to a complete explanation of the mediator pattern, it has also provided its implementation, in different programming languages.
</blockquote>

## MediatR Library
The MediatR library is an open source project that is implementation of the Mediator pattern in .NET that supports Request and Command. In MediatR, each request is placed in a queue in the memory and then connected to the Handler corresponding to that request. MediatR has no dependencies on other libraries and uses generics in C# to create Response and also to determine Handlers. Another very good feature of MediatR is the support of the Pub/Sub template, by which you can create a notification and call this notification in different places of the project.

<a target="_blank" href="https://github.com/jbogard/MediatR">**Here**</a> you can see the link to the MediatR project on github.

<blockquote class="yellow"><b>NOTE:</b> In this article, we don't intend to use the Pub/Sub feature and we will focus mostly on Request and Command.</blockquote>

## Sample Implementation
In this part, a very demonstartion program will be writen to show you how to use MediatR library to implemented mediator pattern in .Net application. Our example is a web API example that lists products and also finds a product based on its id. In this example, it has been tried to implement GetAllProducts and GetProductById operations separately and in two different handlers. Then mediatR will map the requests to the corresponding handlers for us. This project implemented via .NET 6.0 and C# programming language. You can use VSCode, Visual Studio, or any other IDE or text editor. 

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
<a target="_blank" href="https://github.com/ziagham/MediatR-Demo">**Here**</a>, you can find the source code of this project on Github.

## Conclusion
The mediator pattern is one of the patterns that reduce the complexity and dependency between components. In this article, this pattern (mediator pattern) and its advantages and disadvantages were investigated. In the following, the MediatR library was introduced and it was shown how implementing the Mediator pattern using the MediatR library is a very easy and hassle-free task, which, in addition to providing a clean solution, helps a lot to create loose coupling between different components. The given example was implemented within an ASP.NET WebApi project.