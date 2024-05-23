# ObjectScript Multidimensional Arrays

ObjectScript, commonly used with InterSystems' Caché and IRIS Data Platforms, supports the creation and manipulation of multidimensional arrays. These arrays are highly flexible and are often used to store complex data structures.
Basic Structure

In ObjectScript, a multidimensional array is essentially an associative array (or dictionary) where each element can be an array itself. This allows for the creation of nested structures.

## Example 1: A very simple multidimensional array

First, a simple example of how these arrays work:

```
set a(1) = "one"
set a(2) = "two"
set a(3) = "three"
```

Here the array `a` has strings stored at the associated indices. You can access an individual index and it's value with the following. If you attempt to access an index that doesn't exist, you get an undefined error (output indicated with ">").

```
write a(2)
> two

write a(5)
> WRITE a(5)
> ^
> <UNDEFINED> *a(5)
```

You can append extra dimensions to existing indices with this syntax. The entire array then looks like:

```
set a(1, "fr") = "un"
set a(2, "fr") = "deux"
set a(3, "fr") = "troix"

zwrite a
> a(1) = "one"
> a(1, "fr") = "un"
> a(2) = "two"
> a(2, "fr") = "deux"
> a(3) = "three"
> a(3, "fr") = "troix"

write a(1,"fr")
> un
write a(3,"fr")
> troix
write a(2)
> two
```

You can also test whether data exists at a specific index. Resultant booleans are represented as 1 (true) or 0 (false).

```
write $data(a(1,"fr"))
> 1
write $data(a(1,"jp"))
> 0
```


## Example 2: Another simple multidimensional array

Here's another basic example:

```
// Define a simple 2D array
set ^people("John", "age") = 30
set ^people("John", "gender") = "Male"
set ^people("Sian", "age") = 25
set ^people("Sian", "gender") = "Female"

// Accessing elements
write ^people("John", "age")   // Output: 30
write ^people("Sian", "gender") // Output: Female
```

### Notes

* ^people("John", "age") = 30: This sets the age of John to 30.
* ^people("Sian", "gender") = "Female": This sets the gender of Sian to Female.
* Accessing elements is done using the same syntax: ^people("John", "age") retrieves the value associated with John’s age.

## Example 3: Iterating over a multidimensional array

To iterate over a multidimensional array, you can use the for loop with the order function:

```
// Iterate over the first dimension (names)
set name=""
for {
    set name=$order(^people(name))
    quit:name=""
    write "Name: ", name, !
    
    // Iterate over the second dimension (attributes)
    set attr=""
    for {
        set attr=$order(^people(name, attr))
        quit:attr=""
        write " ", attr, ": ", ^people(name, attr), !
    }
}
```

### Notes

* The outer for loop iterates over the first dimension (names).
* The inner for loop iterates over the second dimension (attributes) for each name.
* $order(^people(name)) gets the next key in the ^people array.
* $order(^people(name, attr)) gets the next key in the sub-array for the given name.

## Example 4: Nested multidimensional arrays

You can nest arrays within arrays to create more complex structures:

```
// Define a nested array structure
set ^company("IT", "Development", "John") = "Developer"
set ^company("IT", "Development", "Sian") = "Tester"
set ^company("HR", "Recruitment", "Alice") = "Manager"
set ^company("HR", "Recruitment", "Bob") = "Assistant"

// Accessing nested elements
write ^company("IT", "Development", "John")  // Output: Developer
write ^company("HR", "Recruitment", "Alice") // Output: Manager
```

### Notes

* ^company("IT", "Development", "John") = "Developer": This sets the role of John in the IT Development department.
* Accessing elements follows the same nested key structure: ^company("HR", "Recruitment", "Alice") retrieves the role of Alice in HR Recruitment.

## Example 4: Removing Elements

To remove elements from a multidimensional array, use the kill command:

```
// Removing an element
kill ^people("John", "age")

// Verify removal
write $data(^people("John", "age")) // Output: 0 (element does not exist)
```

### Notes

* kill ^people("John", "age"): This removes the age attribute for John from the array.
* $data(^people("John", "age")): This checks if the element exists. It returns 0 if the element does not exist.