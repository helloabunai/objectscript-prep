# Objectscript syntax

Listing examples of syntax in Objectscript as I've been working through documentation and playing around. Not super thorough or detailed and probably will be added to in perpetuity.

## Basic Syntax

Variables and Assignment (output denoted with `>` as usual):
    
```
SET x = 10
WRITE x
> 10
```

## Comments:

Single-line comments start with a double slash (//).
Multi-line comments are enclosed in /* ... */.

```
// This is a single-line comment
/* This is a 
   multi-line comment */
```

## Control Structures:

### IF statements

```
IF x = 10 {
    WRITE "x is 10", !
} ELSE {
    WRITE "x is not 10", !
}
```

### FOR loops

```
FOR i=1:1:5 {
    WRITE "i=", i, !
}
```

### WHILE loops

```
SET y = 1
WHILE y <= 5 {
    WRITE "y=", y, !
    SET y = y + 1
}
```

## Functions and Procedures:

Defining and calling functions and procedures

```
MYFUNC(a, b) PUBLIC {
    QUIT a + b
}

SET result = ##class(%classnamehere).MYFUNC(5, 10)
WRITE result
> 15
```

## Data Structures

### Arrays:

ObjectScript supports both one-dimensional and multi-dimensional arrays. We covered this in it's own file so read that.

```
SET arr(1) = "one"
SET arr(2) = "two"
WRITE arr(1)
> one
```
```
SET arr("name", "first") = "John"
WRITE arr("name", "first")
> John
```

### Lists:

Lists are another important data structure.

```
SET list = $LISTBUILD("apple", "banana", "cherry")
WRITE $LISTGET(list, 2)
> banana
```

## String Operations

### Concatenation:

Using the underscore (_) operator.

```
SET str1 = "Hello"
SET str2 = "World"
SET str3 = str1 _ " " _ str2
WRITE str3
> Hello World
```

### String Functions:

ObjectScript provides various string functions like $LENGTH, $FIND, $EXTRACT, etc.

```
SET s = "ObjectScript"
WRITE $LENGTH(s)
> 12
WRITE $EXTRACT(s, 1, 6)
> Object
```

## Error Handling

### Try/Catch:

Error handling is done using TRY and CATCH blocks.

```
TRY {
    SET x = 10 / 0
} CATCH ex {
    WRITE "Error: ", ex.Name, !
}
```

## Object-Oriented Programming

### Class Definition:

Classes and methods are defined within a Class block. Similar to our basics file so not going into detail.

```
Class MyClass {
    Property Name As %String;
    
    Method SayHello() {
        WRITE "Hello, ", ..Name, !
    }
}


SET obj = ##class(MyClass).%New()
SET obj.Name = "Alice"
DO obj.SayHello()
> Hello, Alice
```

### Inheritance:

Classes can inherit from other classes. See our basics file for the difference between properties and parameters.

```
Class Person {
    Property Name As %String;
    
    Method Greet() {
        WRITE "Hello, ", ..Name, !
    }
}

Class Student Extends Person {
    Property StudentID As %Integer;
    
    Method ShowID() {
        WRITE "ID: ", ..StudentID, !
    }
}

SET student = ##class(Student).%New()
SET student.Name = "Bob"
SET student.StudentID = 123
DO student.Greet()
> Hello, Bob
DO student.ShowID()
> ID: 123
```

## Miscellaneous

### Namespaces:

Namespaces are used to manage different databases or logical environments.

```
WRITE ##class(%SYS.Namespace).Name()
> outputs current namespace name
```

### System Functions:

ObjectScript provides a variety of system functions.

```
WRITE $ZVERSION
> Outputs the version of the InterSystems IRIS instance
```

See the docs for more info on system functions. [Link here.](https://docs.intersystems.com/iris20241/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_VARIABLES)
Read the docs a lot, basically.
