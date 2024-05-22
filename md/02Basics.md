# Basic connection / syntax

Extremely basic syntax examples when utilising objectscript from an IRIS terminal.

Assuming your server is running and your VSCode is connected to the server/namespace, within a terminal on your local machine (replacing NAMESPACE_HERE with your target IRIS namespace):

```
<redacted command to login to docker/server>
iris terminal iris -U NAMESPACE_HERE
```

Note the command `iris terminal` should be executed within your docker container/dev server.

You'll now be active in a IRIS terminal session, able to access functions etc from your compiled *.cls code files.

# 1. Simple Objectscript file

Create a file called whatever, but in my case, `example.hello.cls`

```
Class example.hello
{

    ClassMethod MethodName()
    {
            write "hello world", !
    }

    ClassMethod PassParameterExample(name As %String) As %String
    {
        return "returning example" _ name
    }

}
```

## MethodName()
Here we are defining a class called 'example.hello'. It contains two ClassMethods, which are different from regular Methods (more on that in the following example).

The first ClassMethod, MethodName(), writes the string "hello world" as output, and then appends it with a new line; the syntax for which is `, !` in Objectscript.

## IRIS Syntax

Some commands that can then utilise this ClassMethod on your IRIS terminal session:

```
do ##class(example.hello).MethodName()
```

As this ClassMethod does not have a return statement, but a literal `write` command, we can use it within IRIS with `do`. The syntax breakdown is:

```
do                          -       iris command
##class(example.hello)      -       the class we wish to interact with
.MethodName()               -       the ClassMethod to call
```

If you attempt to interact with this ClassMethod with another IRIS command, e.g. `write`, you will see:

```
write ##class(example.hello).MethodName()
hello world

WRITE ##CLASS(example.hello).MethodName()
^
<COMMAND> *Function must return a value at zMethodName+1^example.hello.1
```

The ClassMethod is called, and the `write` statement within is executed, but you receive an error as your IRIS session is expecting a function called with `write` to have a return statement.

The trace at `zMethodName+1^example.hello.1` relates to the transpiled MUMPS code and can be used for debugging if required.

## PassParameterExample()

The second ClassMethod, PassParameterExmaple, takes an input `name` and concatenates the input to "return example"; the syntax for which is `"str" _ <str>`.

Note that this ClassMethod defines the input parameter `As %String`, and returns in the same manner. However, these types are purely for the human behind the keyboard, and are not enforced in any way by the transpiler or compiler.

## IRIS Syntax

Some commands that can then utilise this ClassMethod on your IRIS terminal session:

```
write ##class(example.hello).PassParameterExample("blorp")
returning exampleblorp
```

As this ClassMethod contains a return statement, we use `write`. Using `do` has no effect (I have left the namespace prompt `ex_nmpsc` in for clarity):

```
ex_nmspc>do ##class(example.hello).PassParameterExample("slorp")

ex_nmspc>
```

If you encounter an error which places you into a trace/subshell, you can return to the top-level IRIS shell with `halt`.

# 2. Further Objectscript example

Continuing with examples, another file called `example.hello2.cls`

```
Class example.hello2 Extends %RegisteredObject
{

    Property Name As %String [ InitialExpression = "initval", Private ];

    Parameter GREETING = "HelloParameter";

    ClassMethod SeeGreeting()
    {
        write ..#GREETING , !
    }

    Method SeeProperty()
    {
        write "hi from SeeProperty()" , !
        write ..Name, !
    }

    Method %OnNew(name As %String = "") As %Status
    {
        if (name'="")
        {
            set ..Name=name
        }
        write "OnNew test"
        return $$$OK
    }

}

```

## Class differences

The class `example.hello2` differs from the prior example in a few ways. The class extends `%RegisteredObject`, which allows it to inherit methods from that 'vanilla' package. See [this link](https://docs.intersystems.com/irislatest/csp/documatic/%25CSP.Documatic.cls?LIBRARY=%25SYS&CLASSNAME=%25Library.RegisteredObject) for more info. 

It contains ClassMethods and Methods, which have important distinctions. It also contains two further declarations: a `Property` and a `Parameter`.

### Methods 

Methods are used for instance-specific operations and can access and modify the properties of the instance they belong to.

### ClassMethods 

Class Methods are used for class-level operations that do not rely on instance-specific data, and are called on the class itself.

### Properties

Properties in ObjectScript are used to define the data members within a class. They represent the attributes of an object and are essentially variables associated with an object instance. Properties can have data types, initial values, and constraints. In our example, we are defining a property `Name` as a string, with an initial value of `"initval"`, and set to private (only accessible by this class/object).

Properties are specific to each object/instantiation of the class.

### Parameters

Parameters in Objectscript are class-level constants (note the upper-case of the parameter name). They are used to define static values that are not tied to any specific instantiation of the class, but are constant across all instantiations. 

## SeeGreeting()

This ClassMethod writes the value of our class-parameter 'GREETING', which is accessed in Objectscript with the syntax `..#PARAM` (akin to `this` in other programming languages e.g. python/js/java)

It is then appended with a newline, with `, !` as before. Note that because this is a `ClassMethod` and not a `Method`, it can access the static class parameter `GREETING`.

## SeeProperty()

This Method must be called by an instatiation of the class / from an object. This is done by invoking the `%New()` method which we inherit from our class extension. The method `%New()` has a call-back `OnNew` which we can customise, and will explain later.

```
set eg_object=##class(example.hello2).%New()
do eg_object.SeeProperty()
hi from SeeProperty()
initval
```

Here you can see the object we create (`eg_object` of our class `example.hello2`), and run the Method `SeeProperty` on that instantiation. Following the code, the output shows a string from a write command, and a property from another write command.

## Callbacks

As mentioned, when instantiating a class into an object, calling `%New()` has a callback `OnNew`. We can specify behaviour to occur when a new object is created of our class by creating a Method within our class, called `%OnNew`.

```
Method %OnNew(name As %String = "") As %Status
{
    if (name'="")
    {
        set ..Name=name
    }
    write "OnNew test"
    return $$$OK
}
```

This callback always expects a return type of `%Status`. A status is returned with the prefix `$$$`, as seen in `$$$OK`.

In this callback, we pass an argument `name` as a string, with a blank default value. Within the method, we test for the name being not blank. Negative testing is not `!=` but instead `'=`. If this evaluates as true, we then set the property of this object to our current value of `name`.

```
set namedExample=##class(example.hello2).%New("inputName")
> OnNew test
do namedExample.seeProperty()
> hi from SeeProperty()
> inputName
```

In addition to the output from seeProperty, you can also see the output of the write command within %OnNew "OnNew test". Our callback works as this new object writes it's property `name` to be `inputName`, which is what we passed as an argument.

# Useful commands

* zwrite
```
zwrite namedExample
namedExample=1@example.hello2  ; <OREF>
+----------------- general information ---------------
|      oref value: 1
|      class name: example.hello2
| reference count: 2
+----------------- attribute values ------------------
|             (Name) = "inputName"
+-----------------------------------------------------
```

~~ TODO add more ~~