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
