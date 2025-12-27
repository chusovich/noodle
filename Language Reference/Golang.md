## Variables
### Long Notation

```go
var foo int = 9
var bar string = "Hello World"
var arg bool = true

Var (
	myint int = 1
	mystring string = "hello"
	mybool bool = false
)
```

### Short Notation

```go
foo := 9
bar := "Hello World"
arg := true

foo, bar, arg := 9, "Hello World", true
```

## Operators

Most numeric operators are just like you would expect. You can use the "+" operator to concatenate strings

```go
package main 
import "fmt" 
func main() {
	givenName := "John"
	familyName := "Smith"
	fullName := givenName + " " + familyName
	fmt.Println("Hello,", fullName) }
```

### Shorthand Operators

Note that the add/subtract and assign operators work with numeric variables as well as stirngs

- `--` Reduce a number by 1
- `++` Increase a number by 1
- `+=` Add and assign
- `-=` Subtract and assign

### Logical Operators
#### Comparison
- `==` True if two values are the same
- `!=` True if two values are not the same
- `<` True if the left value is less than the right value
- `<=` True if the left value is less or equal to the right value
- `>` True if the left value is greater than the right value
- `>=` True if the left value is greater than or equal to the right value

#### Logical Operators
- `&&` True if the left and right values are both true
- `||` True if one or both the left and right values are true
- `!` This operator only works with a single value and results in true if the value is false

## Zero Values

Each data type has a zero value.

- Booleans: false
- Numbers: 0
- Strings: "" (empty string)
- pointers, functions, interfaces, slices, channels, maps: nil

## Pointers

Declaring a pointer

1. declare a pointer, has the value of nil

```
var foo *int
```

1. create a new memory address

```
foo := new(int)
```

1. create a pointer to an existing variable

```
var foo int = 4
var bar = &foo
```

### De-referencing Pointer
> [!warning]
> It is best practice to make sure the pointer is not null before you try to get the pointer's value

### Functions with Pointers