---
title: Performance of Array vs Generic List in C#
author: Amin Ziagham
date: 2022-10-23
categories: [Fundamental Programming]
tags: [C#, .NET, Benchmark, Performance, Fundamental Programming]
---

Array and list are two structures that are used a lot. According to the type of application, you can choose one of them. As a rule, the list is most desired by .net programmers due to its dynamic nature and ease of use. This article will compare these two structures from different aspects. Also, their performance will be compared to find out which structure is more suitable in terms of performance.

## Array
An array is a sequence of items in which fixed values are stored and all these items must be of the same or homogenous types. In other words, the array can be considered as a set of variables of the same type that are stored in consecutive sections of the memory, and instead of these variables being defined separately and one by one, they are all defined in one place in the array. By default, expanding the size of the array and inserting a new value in a specific location of the array are not part of the array functionality. Hence, it requires additional code, by programmer, to support these functions. Generally, arrays are used where high speed and performance are required.

Below is an example of how to define an array in C#. The size of this array is 10 sections, all of which are int type.

```csharp
int[] numbers = new int[10];
```

## Generic List
Generic List is part of the .NET collection. A .NET collection contains interfaces and classes that define various Collections of objects that are used in the C# language to store heterogeneous elements and objects. The collection is divided into generic and non-generic categories. Non-generic collections include ArrayLists, Sorted Lists, Stacks, Queues, BitArrays, and Hashtables; On the other hand, Generic collections are a set of interfaces and classes which allows developers to define strongly typed collections. The generic collections include HashSet, Dictionary, KeyValuePair, LinkedList, List, etc. In this post, the generic collection, especially the generic list, will be considered.

Generic lists were and still are popular with .NET programmers due to their ease of use. They provide the programmer with the simplest possible insertion and deletion operations as well as other operations related to the array. But at the same time, they have more time complexity than arrays. Even this complexity increases when the generic list does not have capacity.

<blockquote class="yellow"><b>NOTE:</b> In general, generic lists use array concepts behind the scenes.</blockquote>

Below is an example of how to define a generic list in C#. One of the lists has the capacity, 10, as default at initialization, and the other without capacity.

```csharp
// Generic List of an integer with initialized capacity
List<int> numbers = new List<int>(10);

// Generic List of an integer without initialized capacity
List<int> numbers = new List<int>();
```

## Performance Benchmark
In this section, the performance of above-mentioned data structures will be investigated. <a target="_blank" href="https://www.nuget.org/packages/BenchmarkDotNet">**BenchmarkDotNet**</a> library is used for this purpose. Indeed, BenchmarkDotNet is a lightweight, open-source, powerful .NET library that can transform your methods into benchmarks, track their performance, and share reproducible measurement experiments. 

In the code below, we have defined three functions. All three functions perform the same action. All of them add data in the target data structure, as much as the value of the Count variable, and finally return the filled object. The benchmark has been done for two values of 10 and 100.

<script src="https://gist.github.com/ziagham/6a716412344b412810e54db6f99d1be2.js"></script>

The following code snippet also shows the entry point of the program, which is the point of calling and starting the benchmark.
<script src="https://gist.github.com/ziagham/ea0760c6dca5fbc8c17f230b12c07ee2.js"></script>

As you can see, in the below result, the array has a better performance than the generic list in general. For both 10 and 100 capacity, it works better in the terms of adding a new object or element.

{% include image.html url="/assets/img/posts/2022/10/Array_vs_GenericList_Performance_Benchmark.jpg" description="Array vs GenericList performance benchmark"  title="Figure-1: Array vs GenericList performance benchmark" %}

Also, the performance of the generic list with capacity is much better than the generic list without capacity. Below, without capacity mode, the generic list takes a default value as capacity and automatically doubles the capacity whenever it gets close to it. This operation of increasing the capacity behind the scenes, that is, performing some swap and extend operations of the array, will naturally have more time complexity than the state where capacity is defined.

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/ArrayvsList_Performance_Benchmark">**Here**</a>, you can find the related project on Github.

## Conclusion
In this article, the performance benchmark between array and generic list was investigated. According to the obtained benchmark results, it was determined that the array performs better than generic lists in case of adding operation. Arrays are generally used for high-performance and hardware purposes. But working and maintaining them is a bit difficult and requires more experience. But on the other hand, generic lists also have their benefits. Using them, compared to the array, is simple and supports more operators than the array. According to the obtained results, it was obvioud that generic lists can be a better operator if they have the capacity, compared to the case where there is no capacity.