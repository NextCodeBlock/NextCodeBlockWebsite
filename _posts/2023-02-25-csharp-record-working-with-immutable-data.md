---
title: "C# Record: Working with immutable data"
author: Amin Ziagham
date: 2023-02-25
categories: [Fundamental Programming]
tags: [C#, .NET, Record]
description: Working with immutable data is more powerful, often leads to fewer bugs, and forces you to convert objects into new objects instead of modifying...
---

Working with immutable data is more powerful, often leads to fewer bugs, and forces you to convert objects into new objects instead of modifying existing ones. F# developers are used to this, because F# treats everything as immutable by default. Now we have immutable types in C# as well. In C# 9.0, the record type was added to the language, which makes it easier for you to work with immutable data in C#. But before going into the description of this new type, we will first explain what Immutable objects are and what their advantages are.

## What is Immutable object
An immutable object is said to be that its state cannot be changed after its initialization. Also, a class is called Immutable if all instances created from it are also Immutable. We are using an example of such an object from .NET 1.0. Strings in .NET are immutable and any change to them will create a new string (a new object). The advantages of Immutability in the code include the following:

- Immutable objects are thread-safe, which greatly simplifies concurrent and parallel programming; Because several threads can work with an object whose access is read-only.
- Immutable objects are protected from side effects, such as their changes in different methods. You can pass them to any method and be sure that the object hasn't changed when you're done.
- Working with immutable objects enables memory optimization. For example, the .NET runtime maintains a hash of program-defined threads behind the scenes to ensure that no extra memory is allocated for duplicate threads. Another example is displaying the letter "a" in an editor or display. Once an Immutable object containing the information of the letter "a" is created, this single instance can easily be reused to display thousands of letters "a" without worrying about the high memory consumption of the program.
- Working with Immutable objects results in fewer bugs; Because it is always possible to change the internal state of an object by different parts of the program, it can lead to unwanted bugs.
- Hash lists, which are widely used to improve the efficiency of programs, can be formed based on immutable keys.

## Immutable class
By using **init-only** properties, you can get immutable properties in your class. For instance, in the Customer class, in the following code snippet, all its properties are init-only and therefore immutable. 
<script src="https://gist.github.com/ziagham/ac03e8a83a07dcba167313f6c54f4333.js"></script>
This means that you cannot change the property value when working with this class. If you need to change something, you need to create a new Customer object with the updated data. This is what you do when working with immutable data. Instead of changing an object over time, you create a new object when you need to change it. This means that your object represents the state of the data at a particular point in time.

Below is an example of how to do this. Let's create a Customer object like below:
<script src="https://gist.github.com/ziagham/f9a2436cbb6742f9aa5a35ce4f1dbd93.js"></script>
Now let's assume that at some point in your application you need to change the FirstName of the customer object to Martin. As you work with immutable data, you cannot change the FirstName property. Instead, you create a new Customer object that represents the new state. You might create that new Customer object like the following code snippet. 
<script src="https://gist.github.com/ziagham/c64ff8f03844a87db4420c5e78644b70.js"></script>
Note how the value of the LastName property from the first Customer object is assigned to the value of the LastName property of the second Customer object. But this approach is annoying when you have more features. Of course, you can implement some copy logic with reflection or serialization, and you can also use the auto-mapping library. But C# 9.0 has a better way to work with immutable data classes: **Records**.

## Create your first record
To change the Customer class to a record, you use the record keyword instead of the class keyword. Below you see the corresponding type as a record type:
<script src="https://gist.github.com/ziagham/f35f21e52cdba43bf68f7ccb61fef753.js"></script>

### Create copies of recordd types
The with-expression allows you to create a new object more efficiently. You see it in action in the code snippet below.
<script src="https://gist.github.com/ziagham/45b47430da8061f897ff9e73e6fff598.js"></script>
The above code snippet uses the with-expression to create a new Customer object from the existing Customer object stored in the customer variable. You can read this command like this: Use the value of the existing Customer object properties stored in the customer variable to create a new Customer object, and set the FirstName property of the new Customer object to Martin. Store the new Customer object generated by the with-expression with the values of the properties shown in the comments in the newCustomer variable. But remember, the with statement only works with record types and not with regular classes.

<blockquote class="yellow">
<b>NOTE:</b> When working with immutable data, you create a copy of your object for a change. This method is known as non-destructive modification. Instead of having a single object that represents state over time, you have immutable objects that each represent state at a specific point in time.
</blockquote>

### Check Equality of records types
Records are reference type and value type. Their Equals method is implemented in such a way that it compares all properties values for equality. In fact, the C# compiler creates the Equals method for you, and the compiler also provides overloads for the **==** and **!=** operators, so these operators use the Equal method. This is another feature of record types. This means that you can compare two records by their properties values for equality. The code below shows this in action. First, a Customer object is created and stored in the customer variable. The with-expression then uses the existing customer object to create another Customer object. The value of the FirstName property of the new Customer object is set to Martin. The two Customer objects are then compared with the **==** operator. The result is false because the FirstName property of the new Customer object is not "Jennifer" and its value is "Martin".
<script src="https://gist.github.com/ziagham/ff5415009f3496eabc0379bb129b43a7.js"></script>
After the Console.WriteLine command, the third Customer object is created from the newCustomer object. The with-expression is used to set the value of the FirstName property to "Jennifer"; And the created Customer object is stored in anotherCustomer variable. This means that the object stored in the anotherCustomer variable has the same properties values as the first Customer object stored in the customer variable. Then that first Customer object is compared with the third Customer object stored in anotherCustomer variable with the **==** operator. The result is written back to the console with the Console.WriteLine command. In this case, the result is true because the properties of the two Customer objects contain the same values.

Equality checking is another powerful feature of record types. By calling the Equals method or using the == operator, we compare all the properties values. In fact, record type implements the IEquality<T> interface.

### Output the record
C# compiler for records, behind the scenes, overewrites the ToString method so that after using the ToString method, all the properties related to the record type will be output. Consider the following example:
<script src="https://gist.github.com/ziagham/e9b2ced65c57cf7cdcb416cb52130321.js"></script>
As you can see, all properties of record type are printed.

## Conclusion
In this article, you learned the record types that were introduced with C# 9.0. They make working with immutable data objects in C# a joy. For F# developers this is nothing new, but for C# developers it is a huge improvement in the language. The with-expression is a powerful and elegant clause available only for records that allows you to create a new instances of a record based on an existing record.. The record type overrides both the ToString and Equals methods on the Object base class. Calling ToString on a record type will display all its properties. Since record types is refrence-type in nature, they are equal when their properties are equal.