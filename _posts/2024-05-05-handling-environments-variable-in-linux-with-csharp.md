---
title: "Handling Environments Variables in linux with C#"
author: Amin Ziagham
date: 2024-05-05
last_modified_at: 2024-05-05
categories: [Operating Systems]
tags: [C#, .NET, OS, Operating System, Linux, Windows, EnvironmentVariable, System.Environment]
description: Get and Set environment variables via System.Environment.GetEnvironmentVariable in Linux are done only at the process level. Then ...
image: '/assets/img/posts/2024/05/Environments-Variables-Open-Graph.jpg'
---

Environment variables, often referred to as ENVs, are dynamic “objects” on a computer that contain editable values. These values can affect the behavior of running processes. When a program runs, it can access environment variable values for configuration purposes. These variables serve as a means to convey essential information to software and shape how they interact with the environment.

One of the problems while working with .NET on linux-based OS is that Get and Set environment variables in Linux OS are done only at the process level. This means that it is not possible to read and write the environment variables. In this article, we explain how we can store and retrieve environment variables at system level in linux OS by using C#.

## System.Environment class
In C#, you can work with environment variables using the System.Environment class. 

### Get EnvironmentVariable
This get the value of an environment variable. The method signature is like **System.Environment.GetEnvironmentVariable (variableName  [, Target])** which allows us to retrieve an environment variable from either the current process or the Windows operating system registry. The second parameter specifies the target and target can have three kinds:

- **EnvironmentVariableTarget.Process:** Retrieves from the current process (For Windows, Linux, macOS).
- **EnvironmentVariableTarget.User:** Retrieves from the user-specific registry key  (Only for Windows).
- **EnvironmentVariableTarget.Machine:** Retrieves from the local machine-specific registry key (Only for Windows).

<script src="https://gist.github.com/ziagham/c0a95c750300be7533c04ac700e87023.js"></script>

### Set EnvironmentVariable
To set an environment variable, use the **System.Environment.SetEnvironmentVariable(variableName, value [, Target])** method. Again, the Target parameter specifies the scope where you want to set the variable. Example:

<script src="https://gist.github.com/ziagham/70a14aa96f18e03000a9d0137907e3ce.js"></script>

According to the above description and EnvironmentVariableTarget, it can be concluded that GetEnvironmentVariable and SetEnvironmentVariable will not work in Linux environment for **User** and **Machine** values. In the following, we will have the relevant implementation and explanations that will give us this ability to read and write environment variables in the Linux environment.

## Managing EnvironmentVariables in linux
In the following, the implementation will be presented, which can also work in linux-based OS with environment variables.
<script src="https://gist.github.com/ziagham/2cbc570d8e7165c76dfe44eef5a5f3bc.js"></script>

As seen in the code above, in both Get and Set methods, the operating system is checked first. If the operating system is Windows, it will use the default System.Environment.GetEnvironmentVariable class. otherwise, it will execute a process to read and manipulate an environment variable.

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/Linux-EnvironmentVariables-Demo">**Here**</a>, you can find the source code of this article on Github.

## Conclusion
In this article, the handling of environments variables was discussed. One of the problems of using the System.Environment.GetEnvironmentVariable class, in C#, within the Linux OS is that you cannot access Environments Variables outside the of current process. Therefore, a simple implementation was presented in this article, so that make us able to access to System Environments Variables in an OS other than Windows.