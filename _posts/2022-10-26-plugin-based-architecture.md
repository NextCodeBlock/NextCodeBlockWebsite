---
title: Plugin-based (MicroKernel) Architecture
author: Amin Ziagham
date: 2022-10-26
categories: [Software Architecture]
tags: [C#, .NET, Reflection, Plugin, MicroKernel, Software Architecture]
description: The general concept and idea of the plugin are straightforward. That is, being able to add (plugin) features to an existing system or component without...
---

This article aims to describe the plugin architecture with an example. First, the concepts and high-level architecture will be described. After that, the given explanations will be implemented with an example.

Many large and widely used systems have used this architecture in their implementation. Software such as Google Chrome (extensions section), VSCode, Eclipse, Wordpress, and many others software. In essence, the Plugin architecture is a mechanism and solution that allows the system to be modular, customizable, and expanded with minimal effort and configuration. Some systems allow the user and programmer to develop their desired plugin for the software system by following certain principles and interfaces. For example, Google Chrome gives users and developers the ability to create and even share their own themes or extensions.

## Concept
The general concept and idea of the plugin are straightforward. That is, being able to add (plugin) features to an existing system or component without that component knowing the code details of those connected plugged-in features. Systems based on plugin architecture consist of two main components: Core system and plugin components, these two components will be described in the following. The main key design here is to allow adding additional features as plugins to the core system.

In the figure below, you can see a schematic view of the plugin architecture. In this figure, there is a core system to which various plugin components are connected.

{% include image.html url="/assets/img/posts/2022/10/Plugin_Architecture_Diagram.jpg" description="Plugin Architecture Diagram" title="Figure-1: Plugin Architecture Diagram" %}

### Core system
At a high level, this layer just defines how the system works and the core business logic. Basically there is no specific implementation and everything is abstracted. Generally, the core system will have a part that loads, recognizes, and manages plugin components. This part is known as plugin-manager or plugin-register. The core system track of plugins through this part, plugin-manager or plugin-register, which includes information like the plugin name, manufacture details, action to be taken, the protocol for accessing it, and etc. Each plugin individually may have errors or problems and fail. Therefore, the core system must be designed in such a way that a defect in a plugin does not affect other system components and other plugins. Also, the core system must perform logging operations for plugins.

### Plugin component
The plugins are independent components that contain specialized processing, custom code, and additional features that are intended to be addedd to the core system to enhance or extend it to produce additional functionality. One of the interesting features of plugins is that they can be added or removed from the core system at any time. The important thing is that plugins should be separate from each other as much as possible and have no connection with each other as minimal as possible.

## Advantages and disadvantage of Plugin Architecture
### Advantages
- **Independence from one another:** As far as the plugins are independent, it is possible to remove, add, and change plugins quickly. Depending on how the pattern is utilized and implemented, each plugin can be implemented, scaled, and tested separately.
- **Extensibility:** The core system can be dynamically extended to include new plugin features.
- **Simplicity:** A plugin usually has one function. Therefore, developers will concentrate more on developing the logic behind that function.
- **Parallel Development:** Since plugins are separate from each other, they can be implemented as separate components. They can be developed in parallel by different teams.

### Disadvantage
- **Maintainability:** Managing versions and backwards compatibility with existing plugins can be very hard. Hard enough that many practical implementations don't bother, and push the onus on plugin writers to update their plugins with each version.
- **Complexity:** Although each plugin works when tested alone, interactions between plugins can cause new problems, with bugs appearing only with certain combinations of plugins.
- **Testing:** Testing plugins can be difficult if the plugin system does not provide some form of mock plugin runner for testing, which is sometimes not possible, and testing is only available by running the plugin for real, which slows down development.

## Sample Implementation
In this part, a very demonstartion program will be writen to show you how plugin architecture implemented in real world. Our example is a program about mathematical calculator that works with commands. It means that we enter the math function name (e.g., Sin, Cos) with a related paramenter, then the system will display the result of that calculation. In the following these ddifferent part of the demo project will be described. This project implemented via .NET 6 and C# programming language. You can use VSCode, Visual Studio, or any other IDE or text editor.

First, we will create a solution and call it **PluginArchitecture**. After that, we add a ClassLib project called **PluginLib** to our solution. Then, add <u>IPlugin.cs</u> interface as following code. All plugins to be defined must implement this interface. This interface is evaluated by the plugin loader to load plugins. In this way, any DLL file that has a class that implements the IPlugin interface will be loaded by the plugin loader.
<script src="https://gist.github.com/ziagham/389a73ad4865951b782c676a488c351a.js"></script>

The next step is to add the <u>PluginLoader.cs</u> class, according to the code below to the PluginLib project. This part is the important part. This class will take a specified path as input, and load all the DLL files in that path. Then it will load those that have a class that inherits from the IPlugin interface and create an instance of them.
<script src="https://gist.github.com/ziagham/7ffb6e65e14947aa66f2ea6006dac988.js"></script>

In the next step, add a console application to the solution. We call this Console project **Application** and make it **Set as Statup project**. In addition, we need to add PluginLib project as reference. Then write the following code in the Program class. The LoadPlugins function loads plugins from the **Plugins folder**. This means there must be a folder named Plugins in the execution path where the plugins will be placed in there. The ExecuteCommand function is also responsible for receiving commands from the user and executing them.
<script src="https://gist.github.com/ziagham/197c1982772540e8e800fdb03fbf0b3d.js"></script>

Next, we add a Classlib project to our solution and name it Mathematical. Then, add PluginLib project as reference. After that, we will add two classes named **Sin.cs** and **Cos.cs** and put the following codes in them.

<script src="https://gist.github.com/ziagham/d049f05df0032f2089025a88473b631b.js"></script>

<script src="https://gist.github.com/ziagham/cdc71e663ef11e4bf30f025db1d2a075.js"></script>

In addition to these two plugins, Sin and Cos, we also added two plugins named **Help** and **Exit** to the Console Application project. Help plugin is in charge of displaying the list of commands and also explanation related to each plugin. Exit plugin is also responsible for exiting the program gracefully.

FInally, try to build the solution. Then go to the executable path and create a folder called Plugins. After that, find the Mathematical.dll file from the build path of the Mathematical project and copy it to the Plugins folder. Go to the Console Application project folder, and run this command:

> dotnet run -c release

after that you will see the somthing like this on your screen.
{% include image.html url="/assets/img/posts/2022/10/Plugin_Architecture_Implementation_Output_Screenshot-01.jpg" description="Running demo application" title="Figure-1: Main screen of running demo application" %}

then type **help** command; you will see the list of available commands (like below figure).
{% include image.html url="/assets/img/posts/2022/10/Plugin_Architecture_Implementation_Output_Screenshot-02.jpg" description="Running demo application" title="Figure-2: Result of executing Help command" %}

in addition, type **sin 30.6** to see the executed result.
{% include image.html url="/assets/img/posts/2022/10/Plugin_Architecture_Implementation_Output_Screenshot-03.jpg" description="Running demo application" title="Figure-3: The result of executing Sin command" %}

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/PluginArchitecture-Demo">**Here**</a>, you can find the source code of this project on Github.

## Conclusion
In this article, the Plugin architecture concepts was explained. In general, the plugin architecture is consist of two part: Core system and Plugin component. Plugin architecture is one of the architectures that gives programs the ability to modularize itself. This important feature, along with several other advantages (e.g., Extensibility, Simplicity, and Parallel Development), make this architecture one of the most popular architectures for large programs (e.g., Chrome, WordPress). But along with these advantages, it also has disadvantages. Disadvantage like Maintainability, and Testing. But its advantages outweigh its disadvantages. In the following, we had a very small implementation of these concepts about mathematical calculator that works with commands. The commands in that demo program were in the form of plugins.