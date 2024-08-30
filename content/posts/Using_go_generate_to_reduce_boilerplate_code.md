+++
title = "Using go generate to reduce boilerplate code"
date = "2023-03-28"
author = "Ukeje Goodness"
description = "Use the go generate command line tool to simplify code generation and improve code maintainability and reusability."
[taxonomies]
tags = ["Go", "development", "Tools", "Technical", "best-practices"]
+++
---

_**[LogRocket](logrocket.com) made this piece possible. They provide AI-first session replay and analytics that shows
you what's wrong.**_


# Using `go generate` to reduce boilerplate code
Boilerplate code is a section of code that is repeated in multiple parts of programs throughout a software application with little to no variation. Boilerplate code is usually necessary for application functionality without directly contributing to its primary purpose or functionality.

Boilerplate code may include operations like setting up basic file structures, initializing variables, defining functions, or importing libraries or modules. In some cases, packages provide boilerplate code as a starting point for developers to build upon, usually by generation after code behavior configurations.

Although boilerplate code may be necessary and valuable for application functionality, it can also be wasteful and redundant. For this reason, there are many tools to minimize boilerplate code.

`go generate` is a command-line tool for the Go programming language that allows for automatic code generation. You can use `go generate` to generate specific code for your project that is easy to modify, making the tool powerful for reducing boilerplate.

Jump ahead in this article:

- Getting started with `go generate`
- Generating Go code with `go generate`
    - The Stringer code generator
- Best practices for using `go generate` to reduce boilerplate code


## Getting started with `go generate`

The `go generate` command allows you to run code generators as part of the `go build` process. Code generators are third-party programs that generate Go code based on specific inputs, such as protobuf files, database schemas, or configuration files. Popular Go packages like [Gqlgen](https://blog.logrocket.com/gqlgen-build-faster-graphql-server/) use Go generate to generate code for the build process.

There are many benefits to using `go generate` for boilerplate code:


1. `**go generate**` ******s****implifies** **c****ode** **g****eneration:** You can automate the process with `go generate` instead of manually running code generators. This method makes it easier to generate code and reduces the chances of errors or typos
2. **Improves** **c****ode** **m****aintainability and** **r****eusability**: By automating code generation, you can ensure that generated code is always up-to-date with the latest changes to your input files, making it easier to maintain your codebase and avoid bugs caused by outdated code. You can also use generated code in multiple projects, making it easier to reuse code and reduce duplication (reducing boilerplate code)
3. **Enables integration with other tools**: Using `go generate` for code generation means allowing you to inter-operate with other tools, such as `go fmt` or `go vet`, to automate common development tasks
4. **Generating code from annotations:** You can use `go generate` to generate code based on annotations in the source code. You can define a custom struct tag to represent a field in a database schema, then use `go generate` to generate code to convert between the struct and the database schema automatically
5. **Generating code from external sources:** You can use `go generate` to generate code from external sources such as API specifications or database schemas. For example, you can use `go generate` to generate client code for a REST API based on its Swagger definition.

`go generate` is built into Go’s toolchain, and starting with the tool is easy. You’ll need to add a special code comment to your Go code that specifies generators and their arguments.

Here’s an example of the special comments you can use with `go generate`:


    //go:generate protoc --go_out=. myproto.proto

The comment specifies that `go generate` should run the `protoc` command with the `--go_out` flag and generate the Go code from the `myproto.proto` file.

You can have multiple `go:generate` comments in a single file; `go generate` will run the comments in the order they appear in the file.

After specifying the comments, run `go generate` in the same directory as your Go file to run the files and generate the code:


    go generate

You can learn more about the `go generate` command-line tool by specifying the help command before the generate command:


    go help generate

The command returns information about `go generate` and the options and functionalities you can explore with the tool:


![](https://paper-attachments.dropboxusercontent.com/s_7E0C046686473433B379C243B74EE69C9295AFB8B0740A9F782B24456ADC1238_1677440152649_Untitled+2.png)



> go generate does not parse the file, so lines that look like directives in comments or multiline strings will be treated as directives.


## Generating Go code with `go generate`

You can use many code generators for your Go programs, each with their own pros and cons.

Here’s an overview the Stringer generator; one of the most popular code generators in the Go ecosystem.

**The** **Stringer** **code generator** 
Stringer is a code generator that automatically creates string methods for Go types. The generated methods are used to convert the values of the type to strings, which can be helpful for debugging and printing output.

Add this line of comment to your code to use the Stringer generator:


    //go:generate stringer -type=MyType

The comment specifies that `go generate` should run the Stringer generator with the `MyType` type. After generating, the argument must be executable with the directive to perform an action.

Here’s an example of how you can use `go generate` to generate boilerplate code with the `stringer` code generation tool.

 `stringer` implements a `String()` method for any type, allowing you to convert the instance of a custom type to a string.

Run this command in the terminal of your current working directory to install the Stringer tool:


    go install golang.org/x/tools/cmd/stringer@latest 

Stringer generates Go code to implement the `Stringer` interface for any type. Here’s an example Stringer interface:


    type Stringer interface {
        String() string
    }

Implementing the interface allows type conversion to string implementation. To use `go generate` with `stringer`, you’ll need to define a type that needs a string representation:


    type Color int
    
    const (
        Red Color = iota
        Green
        Blue
    )

Create a separate Go file and add this `go:generate` directive at the top to specify the command that generates the `stringer` code:


    //go:generate stringer -type=Color
    
    package main

The directive tells `go generate` to run the `stringer` command with the `-type` flag set to `Color`.

Run the `go generate` command in your working directory to generate the resulting file named `colour_string.go` in the same directory containing the implementation of the `String` method for the `Colour` type:


    go generate ./...

The command runs all `go generate` directives in your working directory.

Here’s the code generated by the `stringer` tool:


    // Code generated by "stringer -type=Color"; DO NOT EDIT.
    
    package main
    
    import "strconv"
    
    func _() {
            // An "invalid array index" compiler error signifies that the constant values have changed.
            // Re-run the stringer command to generate them again.
            var x [1]struct{}
            _ = x[Red-0]
            _ = x[Green-1]
            _ = x[Blue-2]
    }
    
    const _Color_name = "RedGreenBlue"
    
    var _Color_index = [...]uint8{0, 3, 8, 12}
    
    func (i Color) String() string {
            if i < 0 || i >= Color(len(_Color_index)-1) {
                    return "Color(" + strconv.FormatInt(int64(i), 10) + ")"
            }
            return _Color_name[_Color_index[i]:_Color_index[i+1]]
    }

The program provides the functionality you need to return the string representation of constant values.

You can now use the `String()` method to convert a `Color` value to a string:


    func main() {
        c := Red
        fmt.Println(c.String()) // Output: "Red"
    }

Here’s the result of accessing the `Red` constant and printing the string representation:

![](https://paper-attachments.dropboxusercontent.com/s_7E0C046686473433B379C243B74EE69C9295AFB8B0740A9F782B24456ADC1238_1677440123894_Screenshot+2023-02-23+at+17.56.04.jpg)


That’s how you can use `go generate` and `stringer` to automate the generation of string methods for custom types to save time and reduce errors.

## Best practices for using `go generate` to reduce boilerplate code

When using `go generate` to reduce boilerplate code, it is important that you adhere to best practices for the best output during code generation.

Here are a few tips you’ll want to consider when using the `go generate` tool:

1. Use `go generate` with other Go tools like `go fmt` and `go vet` to automate common development tasks
2. Ensure that the generated code is updated with the latest changes to your input files for easier maintainability
3. Document the `go generate` directive to prevent confusion
4. Use separate files for the `go generate` directive instead of adding the directives to existing files to keep your code organized and easier to modify
5. Use `go generate` for repetitive code like serializers, deserializers, and mock implementations, and avoid using the tool for dynamic code
6. Always test the generated code to ensure it works as expected to catch bugs and issues early
    
## Conclusion

In this article, you learned about the `go generate` tool and how you can use it to generate code and reduce boilerplate and redundant code in your projects.

`go generate` simplifies code generation, improves code maintainability and reusability, and enables integration with other tools. Using `go generate` with other best practices can reduce boilerplate code and save time while avoiding errors or typos.

