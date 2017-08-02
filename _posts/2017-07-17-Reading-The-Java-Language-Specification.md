---
layout: post
title: Reading The Java Language Specification
comments: true
categories:
- blog
tags:
- system
---

I've been used Java as my first weapon for years, yet I'm alway confused by language details and running internals. Thus I want to take a closer look at the inside of this language, and reading **The Java Language Specification** is a good way. Since Java 8 is popular and widely used now, I choose *Java SE 8 edition* as the standard. This is a start of a series of posts to reading digests and thoughts recording.

<hr>

## Lexical Structure

The Java programming language is a general-purpose, concurrent, class-based, object-oriented language. It is strongly and statically typed.

As for Java, we should clarify compile-time and run-time clearly. Simply, compile time normally consists of translating programs into a machine-dependent byte code representation. Run-time includes loading and linking of the classed needed to execute a program, optional machine code generation and dynamic optimization of the program, and actual program execution.

LineTerminator in Java:
+ the ASCII LF character, also known as "newline"
+ the ASCII CR character, also known as "return"
+ the ASCII CR character followed by the ASCII LF character

WhiteSpace in Java:
+ the ASCII SP character, also known as "space"
+ the ASCII HT character, also known as "horizontal tab"
+ the ASCII FF character, also known as "form feed"

Java letter include uppercase and lowercase ASCII latin letters A-Z(\u0041-\u005a), and a-z(\u0061-\u007a), and for historical reasons, the ASCII underscore(_, or \u005f) and dollar sign($, or \u0024). If a ltter is a Java letter, the method `Character.isJavaIdentifierStart(int)` returns true.

Java digits include the ASCII digits 0-9(\u0030-\u0039). If a letter is a character, the method `Character.isJavaIdentifierPart(int)` returns true.

Reserved keywords:

keyword  | keyword  | keyword    | keyword   | keyword
-------- | -------- | ---------- | --------- | ------------
abstract | continue | for        | new       | switch
assert   | default  | if         | package   | synchronized
boolean  | do       | goto       | private   | this
break    | double   | implements | protected | throw
byte     | else     | import     | public    | throws
case     | enum     | instanceof | return    | transient
catch    | extends  | int        | short     | try
char     | final    | interface  | static    | void
class    | finally  | long       | strictfp  | volatile
const    | float    | native     | super     | while

The `strictfp` keyword is not so commonly used. It is used to enforce the JVM to return exactly the same results from floating point calculations on every platform. With `strictfp` your results are portable, without it they are more common likely to be accurate. Please see more [here](https://stackoverflow.com/questions/517915/when-should-i-use-the-strictfp-keyword-in-java).

<hr>

## Types

The Java programming language is a *statically typed* language, which means that every variable and every expression has a type that is known at compile time. The types of Java are divided into two categories: **primitive types** and **reference types**.

### Primitive Types

*PrimitiveType*:    
    *{Annotation} NumericType*    
    *{Annotation} boolean*

*NumericType*:    
    *IntegralType*    
    *FloatingPointType*

*IntegralType*:    
    byte short int long char

*FloatingPointType*:    
    float double

Primitive values do not share state with other primitive values. The integral types are byte, short, int, and long, whose values are 8-bit, 16-bit, 32-bit and 64-bit signed two's-complement integers, respectively, and char, whose values are 16-bit unsigned integers representing UTF-16 code units. The floating-point types are float, whose values include the 32-bit IEEE 754 floating-point numbers, and double, whose values include the 64-bit IEEE 754 floating-point numbers.

### Reference Types

*ReferenceType*:    
    *ClassOrInterfaceType*    
    *TypeVariable*    
    *ArrayType*    

*ClassOrInterfaceType*:    
    *ClassType*    
    *InterfaceType*    

*ClassType*:    
    *{Annotation} Identifier [TypeArguments]*    
    *ClassOrInterfaceType . {Annotation} Identifier [TypeArguments]*

*InterfaceType*:    
    ClassType

*TypeVariable*:    
    *{Annotation} Identifer*

*ArrayType*:    
    *PrimitiveType Dims*    
    *ClassOrInterfaceType Dims*    
    *TypeVariable Dims*

*Dims*:    
  ...*\{Annotation\}* [ ] *\{\{Annotation\}* []*\}*

An object in Java is a class instance or an array. Reference values are pointers to these objects, and a special null reference, which refers to no object. The operators on references to objects are:
+ Field access, using either a qualified name or a field access expression.
+ Method invocation.
+ The cast operator.
+ The string concatenation operator `+`, which, when given a String operand and a reference, will convert the reference to a String by invoking the toString method of the referenced object (using "null" if either the reference or the result of toString is a null reference), and then will produce a newly created String that is the concatenation of the two strings.
+ The `instanceOf` operator.
+ The reference equality operators `==` and `!=`.
+ The conditional operator `?` `:`.

### The Class `object`

The class `object` is the superclass of all other classes. All classes and array types inherit the methods of class `object`.

+ `clone`: used to make a duplicate of an object.
+ `equals`: used to define a notion of object equality.
+ `finalize`: runs before an object is destroyed.
+ `getClass`: returns the `Class` object that represents the class of the object. A `Class` object exists for each reference type. A class method that is declared `synchronized` synchronizes on the monitor associated with the `Class` object of the class.
+ `hashCode`: returns an int as a hash code of this object.
+ `wait`, `notify`, `notifyAll`: used in concurrent programming.
+ `toString`: returns a `String` representation of the object.

### The Class `String`

Instance of class `String` represent sequences of Unicode code points. A `String` object has a constant value. String literals are references to instances of class String. The string concatenation operator `+` implicitly creates a new String object when the result is not a constant expression.

Two different types are the *same compile-time type* if they have the same binary name and their type arguments are the same. At run time, several reference types with the same binary name may be loaded simultaneously by different class loaders. These types may or may not represent the same type declaration. Even if two such types do represent the same type declaration, they are considered distinct.

Two reference types are the *same run-time type* if satisfied one of the following:
+ They are both class or both interface types, are defined by the same class loader, and have the same binary name.
+ They are both array types, and their component types are the same run-time type.

### Subtyping among Primitive Types
The following rules define the direct supertype relation among the primitive types:

+ `double >1 float`
+ `float >1 long`
+ `long >1 int`
+ `int >1 char`
+ `int >1 short`
+ `short >1 byte`
`>1` means direct supertype.

## Variables

A variable is a storage location and has an associated type, sometimes called its *compile-time type*, that is either a primitive type or a reference type.

There are eight kinds of variables:

1. A `class variable` is a field declared using the keyword `static` within a class declaration, or with or without the keyword `static` within an interface declaration. A class variable is created when its class or interface is prepared and is initialized to a default value. The class variable effectively ceases to exist when its class or interface is unloaded.
2. An `instance variable` is a field declared within a class declaration without using the keyword `static`.
3. `Array components` are unnamed variables that are created and initialized to default values whenever a new object that is an array is created.
4. `Method parameters` name argument values passed to a method. For every parameter declared in a method declaration, a new parameter variable is created each time that method is invoked.
5. `Constructor parameters` name argument values passed to a constructor. For every parameter declared in a constructor declaration, a new parameter variable is created each time a class instance creation expression or explicit constructor invocation invokes that constructor.
6. `Lambda parameters` name argument values passed to a lambda expression body. For every parameter declared in a lambda expression, a new parameter variable is created each time a method implemented by the lambda body is invoked.
7. An `exception parameter` is created each time an expcetion is caught by a `catch` clause of a `try` statement. The new variable is initialized with the actual object associated with the exception.
8. `Local variables` are declared by local variable declaration statements.

### `final` Variables
A variable can be declared as `final`, and it can only be assigned to once. Compile-time error is thrown if a `final` variable is assigned to unless it is definitely unassigned immediately prior to the assignment. Once a `final` variable is assigned, it always contains the same value. For reference type, it always refers to the same object or array, though the object or array may be changed by operations on them.

Three kinds of variables are *implicitly* declared `final`:

1. A field of an interface.
2. A local variable which is a resource of a `try`-with-resources statement.
3. An exception parameter of a multi-`catch` caluse. An exception parameter of a uni-`catch` clause is never implicitly declared `final`, but may be effectively final.

### Initial Values

+ Each class variable, instance variable, or array component is nitialized with a default value when it is created:
    + `byte`: (byte)0
    + `short`: (short)0
    + `int`: 0
    + `long`: 0L
    + `float`: 0.0f
    + `double`: 0.0d
    + `char`: '\u0000'
    + `boolean`: false
    + reference types: `null`
+ Each method parameter is initialized to the corresponding argument value provided by the invoker of the method.
+ Each constructor parameter is initialized to the corresponding argument value provided by a class instance creation expression or explicit constructor invocation.
+ An exception parameter is initialized to the thrown object representing the exception.
+ A local variable must be explicitly given a value before it is used.