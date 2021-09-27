# Important commands while calling go modules locally

## References
1. [Call your code from another module](https://www.markdownguide.org/basic-syntax/)
1. [Other helpful links](https://linguinecode.com/post/how-to-import-local-files-packages-in-golang)

## Example 1
### Create a module that will be called from another module
1. **Create a module directory where exported functions and or variables will be stored**
    ```
    mkdir greetings
    <home>
      |-- greetings
    ```

1. **Create a main module directory that will call/import exported functions and or variables**
    ```
    mkdir hello
    <home>
      |-- hello
    ```

1. **Final directory Structure**
    ```
    <home>
     |-- greetings
     |-- hello
    ```

1. **Start your module using the go mod init command**. Run the go mod init command, giving it your module path -- here, use example.com/greetings. If you publish a module, this must be a path from which your module can be downloaded by Go tools. That would be your code's repository.
    ```
    cd <home>/greetings
    go mod init example.com/greetings
    ```

1. **Sample source code for *welcome.go* file**
    ```
    package greetings 
    func Greeting() string {
	    return "Hello! Its a WONDERFUL world"
    }
   ```

### Call your code from another module
In the sections above, you created a greetings module. In this section, you'll write code to make calls to the `Greeting()` function in the module you just wrote. You'll write code you can execute as an application, and which calls code in the greetings module.

1. **Go to the hello directory for your Go module source code. This is where you'll write your caller**
    ```
    ├── greetings
    │   ├── go.mod
    │   └── welcome.go
    └── hello

    <home>/greetings > cd ../hello
    ```
1. **Enable dependency tracking for the code you're about to write.** To enable dependency tracking for your code, run the go mod init command, giving it the name of the module your code will be in.
    ```
    $ go mod init example.com/hello
    go: creating new go.mod: module example.com/hello
    ```

1. **Create a file hello.go.** Write code to call the Hello function, then print the function's return value
    ```
    package main
    import (
	    "fmt"
	    "example.com/greetings"
    )

    func main() {
	    message := greetings.Greeting()
	    fmt.Println(message)
    }
    ```
1. Edit the example.com/hello module to use your local example.com/greetings module
    ```
    $ go mod edit --replace example.com/greetings=../greetings
    ```
     After you run the command, the go.mod file in the hello directory should include a replace directive
    ```
    module salil.com/hello

    go 1.17

    replace example.com/greetings => ../greetings
    ```
1. From the command prompt in the hello directory, run the go mod tidy command to synchronize the example.com/hello module's dependencies, adding those required by the code, but not yet tracked in the module
    ```
    $ go mod tidy
    go: found example.com/greetings in example.com/greetings v0.0.0-00010101000000-000000000000
    ``` 
1. At the command prompt in the hello directory, run your code to confirm that it works.
    ```
    $ go run .
    Hello! Its a WONDERFUL world
    ```

## Example 2
### Create functions that will be called from your module
1. Create the following directories and files. Create a child directory `myFuncs` under the `proj3` parent directory 
    ```
    ├── proj3
    │   ├── myFuncs   
    ```
1. **Enable dependency tracking for the code you're about to write.** 
    ```
    $ cd proj3
    $ go mod init salilsood.com/sayHello
    ```
    Contents of `go.mod`
    ```
    module salilsood.com/sayHello

    go 1.17
    ```
1. **Create `generalFuncs.go` file in `proj3/myFuncs`**. This will contain the functions you want to call from your module
    ```
    package myFuncs

     import "fmt"

     func MyGreet1() string {
	     return (fmt.Sprintf("Hello! Dear. Welcome!!!"))
     }
     ```
1. **Create `hello.go` in `proj3`** from where you can call your function
    ```
    package main

    import(
      "fmt"
      "salilsood.com/sayHello/myFuncs"
    )

    func main () {
      fmt.Println(myFuncs.MyGreet1());
    }
     ```
1. **Final directory structure with files**
    ```
    ├── proj3
    │   ├── go.mod
    │   ├── hello.go
    │   ├── myFuncs
    │   │   └── generalFuncs.go
    ```
1. At the command prompt in the `proj3` directory, run your code to confirm that it works
    ```
    $ go run .
    Hello! Dear. Welcome!!!
    ``` 