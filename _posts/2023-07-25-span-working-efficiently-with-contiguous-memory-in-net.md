---
title: "Span&lt;T&gt;: Working efficiently with contiguous memory in .NET"
author: Amin Ziagham
date: 2023-07-25
categories: [Memory]
tags: [C#, .NET, Memory, High Performance]
description: Microservices design patterns are software design patterns that generate reusable autonomous services. The aim is to allow developers ...
image: '/assets/img/posts/2023/07/Contiguous-Memory-Span-Open-Graph.jpg'
---

.NET developers are often faced with the challenge of working with large arrays and data structures in a memory-efficient way. Especially in writing performance-critical server applications and scalable cloud-based services that are sensitive to memory consumption. To address these scenarios, .NET introduces flagship type into the ecosystem, namely <b>Span&lt;T&gt;</b>, which is used to provide scalable APIs that don't allocate buffers and avoid unnecessary data copies. This article tries to introduce this .NET feature.

## The problem
When it comes to utilizing various types of memory, C# offers us different kinds versatility. However, the vast majority of developers exclusively utilize the managed one. Let's take a quick look at what C# has to offered us for working with memory:

- **Stack memory:** Allocated using the stackalloc keyword on the Stack. Allocation and deallocation happen quite quickly. The Stack's size is small (often  less than 1 MB) and well suited for CPU cache. However, attempting to allocate more results in a StackOverflowException that cannot be handled and immediately terminates the entire process. For small and short tasks where managed memory cannot be allocated, stackalloc is frequently utilized. 
- **Unmanaged memory:** Invoking the Marshal.AllocHGlobal or Marshal.AllocCoTaskMem procedures will allocate memory on the unmanaged heap (which is hidden from GC). The developer must explicitly release this memory by calling Marshal.FreeHGlobal or Marshal.FreeCoTaskMem. Utilizing it doesn't put the GC under any more pressure. The most frequent usage of Unmanaged memory is to avoid GC in situations where you would typically allocate huge arrays of value types without pointers.
- **Managed memory:** The **`new`** operator can be used to allocate managed memory. It is managed since the Garbage Collector (GC) is in charge of managing it. Developers don't have to worry about it because the GC decides when to free the memory.

The problem is related to when to work with arrays. When it comes to working with arrays and other data collections in C#, developers frequently confront performance and memory management challenges. In particular, copying or allocating extra memory can be a costly operation, especially for large arrays. As an example, consider the following code. An array data structure with 1024 elements and is filled with initial values.

<script src="https://gist.github.com/ziagham/33ac61eb0fe01ef77d51a548efb95502.js"></script>

Now suppose that we want to calculate the sum of values between specific continuous elements (e.g., The third quarter section of the desired array). The general solution for this is to create a new array with 250 elements and put the desired values in it and then perform the sum calculations on the new array (code below).

<script src="https://gist.github.com/ziagham/8bc65138dddcbf0892791dfed706f21b.js"></script>

This way has a problem. The problem is the overhead of reading the original array and copying the desired values to the new array or initialze and new object in memory. This overhead problem will have a direct impact on performance. Especially by increasing the number of elements of the main array or increasing the number of references to the main array. The solution that Microsoft has included in C# is to use Span&lt;T&gt;.

## What is Span&lt;T&gt;?
<b>Span&lt;T&gt;</b> is a value type in .NET within the System namespace that provides a safe and editable view into any arbitrary contiguous block of memory with no-copy semantics. One of the primary advantages of using Span is that it helps you to avoid making duplicate and unnecessary copies of data. When you pass an array to a function, for example, a new copy of the array is created on the heap. This is inefficient, particularly when working with huge and large arrays. You can pass a reference to the original data rather than a copy when using Span, which can enhance efficiency. You can use <b>Span&lt;T&gt;</b> as an abstraction to uniformly represent arrays, strings, memory allocated on the stack, and unmanaged memory. In some ways, it's analogous to C# arrays, but with the added ability to create a view of a portion of the array without allocating a new object on the heap or copying the data. This feature is called slicing and types with this feature are known as sliceable types. Span promises type and memory safety with checks to avoid out-of-bounds access, but that type of safety comes with certain usage restrictions enforced by the C# compiler and runtime. The Figure-1 illustrates a schematic view of how <b>Span&lt;T&gt;</b> select the desired blocks from a contiguous segments in memory.

{% include image.html url="/assets/img/posts/2023/07/Contiguous-Memory-Span.jpg" description="Span&lt;T&gt; provides type-safe access to a adjacent area of memory" title="Figure-1: Span&lt;T&gt; provides type-safe access to a adjacent area of memory" %}

<blockquote class="yellow">
<b>NOTE:</b> <b>Span&lt;T&gt;</b> is introduced in C# 7.2. Furthermore, <b>Span&lt;T&gt;</b> is considered as a build-in library in.NET core 2.1 and higher. Meanwhile, in order to use the span capability in the.NET Framework, you must add the <a target="_blank" href="https://www.nuget.org/packages/System.Memory/"><b>System.Memory</b></a> package from nuget to your code. 
</blockquote>

## Benefits of using Span&lt;T&gt;
- **Reduced GC pressure:** By minimizing the amount of memory allocation and copying, Span&lt;T&gt; can help lessen the burden on the garbage collector (GC), resulting in better performance and less memory fragmentation.
- **Memory safety:** Span&lt;T&gt; provides a type-safe and bounds-checked method of accessing memory, assisting in the prevention of frequent memory-related issues such as buffer overflows and null reference exceptions.
- **Improved performance:** Span&lt;T&gt; can help enhance the efficiency and performance of array and memory manipulation operations by reducing the need to copy or allocate additional memory.
- **Lower memory utilization:** By avoiding the need to copy or allocate additional memory, Span&lt;T&gt; can help minimize the memory usage of application that operate with big arrays or collections of data.
- **Interoperability:** Span&lt;T&gt; can be used to operate with unmanaged memory.
- **Parallelism:** Span&lt;T&gt; can be used to efficiently work with data in parallel.
- **Code simplification:** Span&lt;T&gt; provides a uniform and straightforward approach to work with arrays and memory, minimizing the amount of boilerplate code required for routine tasks.

## Limitations of using Span&lt;T&gt;
- **Boxing:** Span&lt;T&gt; can't be boxed or put on the heap.
- **Reference-type:** Span&lt;T&gt; can't be fields in a reference type.
- **Asynchronous operations:** Span&lt;T&gt; can't be used within asynchronous methods and across await and yield boundaries.
- **Generic-type argument:** Span&lt;T&gt; can't be used as a generic type argument.
- **Type Objects:** Span&lt;T&gt; can't be assigned to variables of type Object, dynamic or to any interface type.

## Important methods and properties of Span&lt;T&gt;
Here is a list of some of the most essential methods and properties accessible in the Span&lt;T&gt; class, along with a brief description:

- **Span&lt;T&gt;.IsEmpty:** Gets an empty Span&lt;T&gt;.
- **Span&lt;T&gt;.Length:** Gets the length of the Span&lt;T&gt;.
- **Span&lt;T&gt;.Slice(int start):** Returns a Span&lt;T&gt; that starts at the specified index and includes all elements to the end of the Span&lt;T&gt;.
- **Span&lt;T&gt;.Slice(int start, int length):** Returns a Span&lt;T&gt; that starts at the specified index and includes the specified number of elements.
- **Span&lt;T&gt;.ToArray():** Copies the elements of the Span&lt;T&gt; to a new array.
- **Span&lt;T&gt;.CopyTo(Span&lt;T&gt; destination):** Copies the elements of the Span&lt;T&gt; to the specified destination Span&lt;T&gt;.
- **Span&lt;T&gt;.SequenceEqual(ReadOnlySpan&lt;T&gt; other):** Returns a value indicating whether the elements of the Span&lt;T&gt; are equal to the elements of the specified ReadOnlySpan&lt;T&gt;.
- **Span&lt;T&gt;.Fill(T value):** Sets all elements in the Span&lt;T&gt; to the specified value.
- **Span&lt;T&gt;.IndexOf(T value):** Searches for the specified value in the Span&lt;T&gt; and returns the index of the first occurrence, or -1 if the value is not found.
- **Span&lt;T&gt;.LastIndexOf(T value):** Searches for the specified value in the Span&lt;T&gt; and returns the index of the last occurrence, or -1 if the value is not found.
- **Span&lt;T&gt;.GetEnumerator():** Returns an enumerator that iterates through the elements of the Span&lt;T&gt;.
- **Span&lt;T&gt;.TryCopyTo(Span&lt;T&gt; destination):** Attempts to copy the elements of the Span&lt;T&gt; to the specified destination Span&lt;T&gt;. Returns true if the operation succeeded, or false if the Span&lt;T&gt; is too large to fit in the destination Span&lt;T&gt;.
- **Span&lt;T&gt;.TryGet(ref T value):** Attempts to get the first element of the Span&lt;T&gt;. Returns true if the operation succeeded, or false if the Span&lt;T&gt; is empty.

## Performance benchmark
In this section, the performance of Span&lt;T&gt; against array manipulation will be investigated. <a target="_blank" href="https://www.nuget.org/packages/BenchmarkDotNet">**BenchmarkDotNet**</a> library is used for this purpose. Indeed, BenchmarkDotNet is a lightweight, open-source, powerful .NET library that can transform your methods into benchmarks, track their performance, and share reproducible measurement experiments.

In the code below, we have defined three functions. All three functions perform the same action. All of them add data in the target data structure, as much as the value of the Count variable, and finally return the filled object. The benchmark has been done for two values of 10, 1000, and 10000.

<script src="https://gist.github.com/ziagham/cfc033f5da0e855f47c7a03ce5c524fe.js"></script>

The following code block also shows the entry point of the program, which is the point of calling and starting the benchmark. Write the blow codew in Program.cs file:
<script src="https://gist.github.com/ziagham/329b68ce81fd680489e880543837a50d.js"></script>

Run the benchmark by entering the following command in the .NET CLI or any other way you prefere. Just keep in mind that you need to run this benchmark as Release configuration, not Debug.

```console
dotnet run -c release
```

After running the benchmark, the following results will be obtained. 

{% include image.html url="/assets/img/posts/2023/07/SpanBenchmark.jpg" description="The performance of Span&lt;T&gt; against LINQ, and array manipulation" title="Figure-2: The performance of Span&lt;T&gt; against LINQ, and array manipulation" %}

<blockquote class="yellow">
<b>NOTE:</b> It is important to mention that the benchmark results obtained is different from one machine to another (according to the specifications).
</blockquote>

As you can see, in the Figure-2, the Span&lt;T&gt; has a better performance and faster than the CopyArray and LINQ expression. For 10, 1000, and 10000 capacity, it works better in the terms of getting memory segments. Also, notice Span&lt;T&gt; has 0 allocation while the other ways have some overhead and this amount will increase as the array capacity increases.

## Project Link
<a target="_blank" href="https://github.com/NextCodeBlock/MemorySpan-Benchmark">**Here**</a>, you can find the related project on Github.

## Conclusion
In C#, the Span&lt;T&gt; type offers a powerful and efficient approach to work with arrays and memory. Span&lt;T&gt; can provide considerable speed gains by avoiding the need to copy or allocate additional memory, especially for large arrays or collections of data. Span&lt;T&gt; can also be used to interoperate with unmanaged memory that expect contiguous blocks of memory. Developers can increase the performance and memory efficiency of their C# programs by employing Span&lt;T&gt;. At the end, a benchmark was conducted between three different methods to calculate the sum of an array for different capacity, and it was found that the Span&lt;T&gt; approach has a better performance in terms of speed and memory allocation.