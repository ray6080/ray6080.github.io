---
layout: post
title: Reading The Java Language Specification
comments: true
categories:
- blog
tags:
- system
---

I've been used Java as my first weapon for years, yet I'm alway confused by language details and running internals. Thus I want to take a closer look at the inside of this language, and the specification is a good way. Since Java 8 is popular and widely used now, I choose Java SE 8 edition as the standard. This is a start of a series of posts to reading digests and thoughts recording.

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

