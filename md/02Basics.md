# Basic connection / syntax

Extremely basic syntax examples when utilising objectscript from an IRIS terminal.

Assuming your server is running and your VSCode is connected to the server/namespace, within a terminal on your local machine (replacing NAMESPACE_HERE with your target IRIS namespace):

```
<redacted command to login to docker/server>
iris terminal iris -U NAMESPACE_HERE
```

You'll now be active in a IRIS terminal session, able to access functions etc from your compiled *.cls code files.

# Simple Objectscript file

Create a file called whatever, but in my case, `example.hello.cls`

```objectscript
Class example.hello
{

    ClassMethod MethodName()
    {
            write "hello world", !
    }

    ClassMethod PassParameterExample(name As %String) As %String
    {
        return "return example" _ name
    }

}
```

notes here

iris terminal commands here

# Further Objectscript example

Continuing with examples, another file called `example.hello2.cls`

```objectscript
Class example.hello2 Extends %RegisteredObject
{

    Property Name As %String [ InitialExpression = "initval", Private ];

    Parameter GREETING = "HelloParameter";

    ClassMethod Hello1()
    {
            write ..#GREETING , !
    }

    Method Hello2()
    {


        write "hello2" , !
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

notes here

iris terminal commands here