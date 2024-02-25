---
title: "Command line (CLI) parsing and invocation in .NET"
author: Amin Ziagham
date: 2024-02-06
last_modified_at: 2024-02-06
categories: [Command Line Interface]
tags: [CLI, Command Line Interface, System CommandLine]
description: The command line is one of the most useful and effective tools available to developers and other computer users. But what is command line interface or CLI? ...
image: '/assets/img/posts/2024/02/System-CommandLine-OG.jpg'
---

The command line is one of the most useful and effective tools available to developers and other computer users. But what is command line interface or CLI? Simply put, the command line interface allows you to run a program or configure a system by entering commands in the terminal. With the help of CLI, you can enter any command as text and see its execution in a fraction of a second. On all systems, including Linux and Windows or macOS, there is a command line interface or CLI in addition to the GUI.

In this article, we explain how we can implement a simple CLI in .NET by using <a target="_blank" href="https://www.nuget.org/packages/System.CommandLine">System.CommandLine</a> package.

## Terminology
I believe a good place to start is to understand what the command line is. You may have heard the terms Terminal, console, command line, CLI, or shell used in this context. People frequently use these terms interchangeably, yet they are actually distinct concepts. Differentiating between them isn't essential knowledge, but it will help clarify things. So lets briefly explain each one.

### Console
The console is a physical device that allows you to interface and interact with your computer. It refers to your computer's screen, keyboard, and mouse. As a user, you interact with your computer via the console.

{% include image.html url="/assets/img/posts/2024/02/Console.jpg" description="Computer Console" title="Figure-1: Computer Console" %}

### Terminal
A terminal is a text-input and output environment. It is a program that functions and acts as a wrapper, allowing us to submit and send commands that the computer will process. It's the "window" where you enter the actual commands that your computer will execute.

{% include image.html url="/assets/img/posts/2024/02/Terminal.jpg" description="Computer Terminal" title="Figure-2: Computer Terminal" %}

Keep in mind that the terminal is just like any other software and programs. And, like any software, you can install and uninstall it whenever you want. It is also possible to have multiple terminals installed on your computer and operate them whenever you wish. All operating systems provide a default terminal, although there are other variations available, each with its own set of functionality and capabilities.

### Command line or CLI
The CLI is the interface through which we enter commands for the computer to process. In plain English, it is the spot where you type the commands that the computer will execute.

{% include image.html url="/assets/img/posts/2024/02/CommandLine.jpg" description="Command line interface (Cli)" title="Figure-3: Command line interface (Cli)" %}

A command marked with a yellow box is considered a cli. This command, `ps -aux`, shows the list of current processes in the system.

## What is System.CommandLine
<a target="_blank" href="https://www.nuget.org/packages/System.CommandLine">System.CommandLine</a> is a.NET package for developing command-line applications that supports POSIX and Windows standards, tab completion, and help text. It allows you to specify commands, options, and arguments, assign them to handlers, and modify the parsing and rendering behavior.

## Key Terms to Understand the System.CommandLine
### Command
A command is an action or verb that the command-line app performs. The very top-level command, the name of your executable (also referred to as arg0), is the root command. A command may also contain subcommands. When running a CLI application, only a single command executes. Using the dotnet CLI as an example:
```console
dotnet publish
```
In this example, `dotnet` is the root command, and `publish` is the executed subcommand.
### Option
An optional named parameter for a command.
```console
dotnet publish --no-build
```
In this example, `--no-build` is an option for the `publish` command.
### Argument
A value passed to either a command or an option. Arguments may have an arity, indicating how many values they can hold.
```console
dotnet publish --configuration Release
```
In this example, `--configuration` is an option to the `publish` command that accepts `Release` as an argument. It only accepts a single value; therefore, it has an arity of one.

## Getting Started with System.CommandLine
To start, we will create a console project in .NET. Then we install the <a target="_blank" href="https://www.nuget.org/packages/System.CommandLine">System.CommandLine</a> package from nuget. Remember to turn "show pre-release" on as the package is still in beta!

We then start by creating a command called *GreetingCommand* and write the below code in it:
<script src="https://gist.github.com/ziagham/8c8c9b7e79951ef6d8b55492284314ca.js"></script>

Then write the following codes in the Program.cs class:
<script src="https://gist.github.com/ziagham/b72f83ad94fe94e8298fe819673a4025.js"></script>

After that, open terminal and navigate to the project path, build the project and run it as below:
```console
dotnet run"
```

we will get the result like below:
{% include image.html url="/assets/img/posts/2024/02/SystemCommandLineDemo-Run-Result.jpg" description="Running result" title="Figure-4: Running result" %}

then try to run the `greeting` command and see the result.
```console
dotnet run -- greeting -n "Amin"
```

And the result will be like below:
{% include image.html url="/assets/img/posts/2024/02/SystemCommandLineDemo-Greeting-Command-Result.jpg" description="Running greeting command result" title="Figure-5: Running greeting command result" %}

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/System-CommandLine-Demo">**Here**</a>, you can find the source code of this project on Github.

## Conclusion
The command line is one of the most valuable and powerful tools available to developers and other computer users. In this article, the Console, Terminal and Command Line Interface (CLI) concepts were explained. In the following this article tried to introduce and describe the System.CommandLine package and how to use it to create a solid and professional command line application.