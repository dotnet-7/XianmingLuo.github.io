---
layout: default
title: Go:Tutorial
---
# Go
## Source Organization
Go code ----> package ----> module \
"In a module, you collect one or more related packages for a discrete and useful set of functions."
## Create Module
"go mod init example.com/greetings" \
why example.com/greetings?

## Function
```
package greetings

import "fmt"

// Hello returns a greeting for the named person.
func Hello(name string) string {
    // Return a greeting that embeds the name in a message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```
### Function Scope (exported name)
"In Go, a function whose name starts with a capital letter can be called by a function **not in the same package**."

### Variable
"In Go, the := operator is a shortcut for **declaring and initializing** a variable in one line." \
Also, type inference based on the value on the right \
Equivalant to \
var message string
message = fmt.Sprintf("Hi, %v. Welcome!", name)

### Function Invocation Out of Package
```
package main

import (
    "fmt"

    "example.com/greetings"
)

func main() {
    // Get a greeting message and print it.
    message := greetings.Hello("Gladys")
    fmt.Println(message)
}
```

## Dependency
### Redirect Dependency
```
go mod edit -replace example.com/greetings=../greetings
```

## Errors
### Throw Errors
```
package greetings

import (
    "errors"
    "fmt"
)

// Hello returns a greeting for the named person.
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message.
    if name == "" {
        return "", errors.New("empty name")
    }
    
    // If a name was received, return a value that embeds the name
    // in a greeting message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil
}
```
### Handle Errors
```
if err != nil {
    log.Fatal(err)
}
```
"If you get an error, you use the log package's Fatal function to print the error and **stop** the program."
## If Condition
```
if name == "" {
    return "", errors.New("empty name")
}
```
## Slice (Array)
### Assignment
```
formats := []string{
    "Hi, %v. Welcome!",
    "Great to see you, %v!",
    "Hail, %v! Well met!",
}
```
### Indexing
```
return formats[rand.Intn(len(formats))]
```

## Loop
### For Loop
```
for _, name := range names {
    }
```
#### range
"Range returns two values, the index of the current item in the loop and a copy of the item's value."
## Built-in Data Structures
### Map
#### Initialization
```
messages := make(map[string]string)
```
#### Indexing
messages[name] = message

## Testing
### Test Source
"Ending a file's name with _test.go tells the go test command that this file contains test functions."
```
package greetings

import (
    "testing"
    "regexp"
)

// TestHelloName calls greetings.Hello with a name, checking
// for a valid return value.
func TestHelloName(t *testing.T) {
    name := "Gladys"
    want := regexp.MustCompile(`\b`+name+`\b`)
    msg, err := Hello("Gladys")
    if !want.MatchString(msg) || err != nil {
        t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
    }
}

// TestHelloEmpty calls greetings.Hello with an empty string,
// checking for an error.
func TestHelloEmpty(t *testing.T) {
    msg, err := Hello("")
    if msg != "" || err == nil {
        t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
    }
}
```
## Compile and Install
### Why need compilation and installation?
"While the go run command is a useful shortcut for compiling and running a program when you're making frequent changes, it doesn't generate a binary executable."
### Build
Generate a executable binary
```
go build
```
### Install
No need to specify path, like grep, find. The binary will be put to the go binary collection directory, which needs to be added to PATH.
```
go install
```

## Multi-module
### Create the workspace
```
go work init ./hello
```
"The use directive tells Go that the module in the hello directory should be main modules when doing a build."
### Add module to workspace
```
go work use ./example
```
"This will allow us to use the new code we will write in our copy of the stringutil module instead of the version of the module in the module cache that we downloaded with the go get command"
"go.work can be used instead of adding **replace** directives to work across multiple modules."

### Other Commands
```
go work use [-r] [dir]
go work edit
go work sync
```

## Struct
### Definition
```
type Vertex struct {
    X int 
    Y int
}
```
### Assignment
#### Struct Assignment
```
v := Vertex{1, 2}
```
#### Member Assignment
```
v.X = 4
```
#### Pointer Deeferencing
```
p := &v
p.X = 1e9
(*p).X = 1e9
```
### Method
#### Definition
```
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```
"A method is a function with a special receiver argument. The receiver appears in its own argument list between the func keyword and the method name."
#### Invocation
```
v.Abs()
```

#### Pointer Receivers
```
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}
```
"Methods with pointer receivers can **modify** the value to which the receiver points. Since methods often need to modify their receiver, pointer receivers are more common than value receivers."

## Interfaces