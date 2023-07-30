---
title: Implement OpenAPI specification for WebApi by Swagger
author: Amin Ziagham
date: 2023-02-06
categories: [Interface Description Language]
tags: [C#, .NET, ASP.NET, OpenAPI, Swagger, RESTful, REST, WebApi,]
description: OpenAPI Specification (OAS) is a language-independent standard that use to define the structure and implementation of RESTful APIs...
image: '/assets/img/posts/2023/02/OAPI-OG.jpg'
---

Today, APIs have greatly influenced the web. The stronger the API documentation is, the better and more useful the API will be. A good API can become a useless tool when people don't know how to use it. This is where it becomes clear why documentation can be essential for a professional and business API. On the other hand, it can be challenging to create and maintain a good document that is easy to understand, engaging with, and pleasing to the consumer. Therefore, there is a need for a standard that can create integrated documents, with the least effort, which, in addition to being cheaper and easier to maintain, can be understood by humans and computers. One of the prominent standards in Restful documentation is OpenAPI Specification. In this article, an explanation has been given about open API specifications. The swagger tool is also introduced in this regard, and finally, an example with .NET will be provided that shows the use of open API specifications by swagger.

## What is OpenAPI Specification
OpenAPI Specification (OAS) is a language-independent standard that use to define the structure and implementation of RESTful APIs, which is independent of the programming language. In the file written with this standard, all the RESTful APIs of the program are described in such a way that humans and computers understand program performance easily without the need to read the program code, without having program documentation, and without monitoring and filtering network traffic. OAS makes the process of calling services and sending requests much simpler. With this anyone who wants to use the program can communicate with it remotely without getting involved in the complexities of implementing the program.

These file specify the necessary items to use your RESTful APIs, including:
- Describe and explain how the API works.
- What should be the values sent (Request) and their type and how to send.
- What are the response values and the description of these values.
- Specifying the method of authentication to send requests.
- Things like contact information, terms of use, license and more.

You can read any information you want about OpenAPI documentation in its <a target="_blank" href="https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.3.md">**Github**</a>.

## What is Swagger
Swagger is a series of open-source programs and tools that are produced based on the OpenAPI specification standard, and with their help, you can design, build, document, and test your REST APIs. Indeed, Swagger is a framework created with the aim of defining a language-agnostic interface to REST APIs. This framework allows the computer and the person to access the services' capabilities without accessing the source code, documents, or internet traffic. If Swagger is defined correctly, the user can notice the remote service and interact with it using the least amount of logic implementation. Swagger takes the guesswork out of calling a service (like interfaces do for lower-level languages). More specifically, we can say that Swagger is an efficient and formal tool that is surrounded by a large group of other tools and includes all parts of the front-end user interface, libraries that include low-level code, and business and management APIs.

The Swagger toolset includes:
- **Swagger Editor:** The Swagger Editor allows you to edit the OpenAPI standard in YAML in your browser and view the documentation in real-time.
- **Swagger UI:** A collection of HTML, JavaScript, and CSS tools that dynamically generate appropriate documentation from an OAS-compliant API.
- **Swagger Codegen:** According to the OpenAPI Spec, it is possible to automatically generate API client libraries (SDK generation), microservices, and documentation.
- **Swagger Parser:** is a standalone library for parsing OpenAPI definitions from within Java.
- **Swagger Core:** Java-related libraries for creating and consuming and working with OpenAPI definitions.
- **Swagger Inspector (Free):** API testing tool that allows you to validate your APIs and create OpenAPI definitions from an existing API.
- **SwaggerHub (Free and Commercial):** API design and documentation built for teams working with OpenAPI.

## Sample Implementation
In this part, a very demonstartion program will be writen to show you how use OpenAPI in real application. Our example is a web API project that doing CRUD on the  product entity. This project implemented via .NET 6.0 and C# programming language. You can use VSCode, Visual Studio, or any other IDE or text editor. 

First, create a web API project. You can do this using Visual Studio or use .NET CLI for it. Then, install the <a target="_blank" href="https://www.nuget.org/packages/Swashbuckle.AspNetCore">**Swashbuckle.AspNetCore**</a> library from Nuget. You can do this by using .NET cli with the following commands.

```console
dotnet add package Swashbuckle.AspNetCore
```

The next step is to use the Swashbuckle.AspNetCore library and implement Swagger methods in the project. To do this, first add the following piece of code to the ConfigureServices method of your project.
<script src="https://gist.github.com/ziagham/a7582c8b5a5c1cd2dfdb01ae7509cb0e.js"></script>
Then add the following two lines to the Configure section of your project.
<script src="https://gist.github.com/ziagham/d919e2bbcaf9f1720f1d59929902fbb1.js"></script>

Build and run your project. After running your WebApi application, you will see an environment similar to the image below (Figure-1). This environment indicates that your api is documented. Swagger displays the methods defined and present in your RESTful api. Click on any method, you can get the details of how to call it and also call it.

{% include image.html url="/assets/img/posts/2023/02/DefaultSwagger.JPG" description="Swagger documentation of your RESTful Api" title="Figure-1: Swagger documentation of your RESTful Api" %}

### Try it out
Now, if you click on one of the displayed items (for example, GET ​/Product), a form similar to the image below will be displayed. 
{% include image.html url="/assets/img/posts/2023/02/Tryitout_GetMethod_Swagger.JPG" description="Try to execute Get method of Product Api" title="Figure-2: Try to execute Get method of Product Api" %}

Click on the **Try it out** button and then press the **Execute** button. After execution, you will see the result like Figure-3.
{% include image.html url="/assets/img/posts/2023/02/Execute_GetMethod_Swagger.JPG" description="Executed Get method of Product Api" title="Figure-3: Executed Get method of Product Api" %}

### Personalize the Swagger UI
You can also customize the information displayed in the swagger documentation. For example, you can add contact details so that customers or developers can contact you if there is a problem. To do this, change the AddSwaggerGen method as follows.
<script src="https://gist.github.com/ziagham/f90274794c8c0275b538848ad5e32436.js"></script>
After that, change the UseSwaggerUI method as follows.
<script src="https://gist.github.com/ziagham/6b3946d587b33497b10c6ba26eccf4f1.js"></script>

Build and run your project. After running, you will see an environment similar to Figure-4. As you can see, the inserted details are shown.
{% include image.html url="/assets/img/posts/2023/02/CustomizeSwagger.JPG" description="Customized Swagger documentation of your RESTful Api" title="Figure-4: Customized Swagger documentation of your RESTful Api" %}

## Demo Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/OpenApi-Swagger-Demo">**Here**</a>, you can find the source code of very simple project and a demonstration of a OpenAPI specification by Swagger on Github.

## Conclusion
Documentation of RESTful API is important. The better the API documentation is, the better the API will be. On the other hand, API documentation can be time-consuming and challenging. Therefore, the OpenApi Specification (Standard) was created to simplify this task so that the focus is more on the development of business code. One of the most prominent tools, that implement the open API standard well, is Swagger. With Swagger, you can easily design, build, document, and test your API. In the end, how to implement and document the API project was described, and the simple project link was also added.