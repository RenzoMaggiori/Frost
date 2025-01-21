# examples

This directory contains examples of how to use the `frost` programming language.
In order to run the examples, you must have the `frost` compiler installed on
your system. You can find instructions on how to install the `frost` compiler in
the
[Getting Started](https://renzo.gitbook.io/frost/user-manual/getting-started)
section of the user manual.

Each file in this directory is a standalone example that demonstrates a specific
feature of the `frost` programming language.

## List of Examples

- [Array](array.ff): This example demonstrates how to access array elements.
- [3D Spinning Donut](donut.ff): This example demonstrates how to generate a
  spinning 3D donut using ASCII characters.
- [Hello](hello.ff): This example demonstrates how to print "Hello, World!".
- [Julia Set](julia.ff): This example demonstrates how to generate an animated
  Julia set.
- [Mandelbrot Set](mandelbrot.ff): This example demonstrates how to generate an
  snapshot of the Mandelbrot set.
- [Putstring](putstring.ff): This example implements the `putstring` function
  that prints a string to the terminal.
- [Sierpinski Triangle](sierpinski.ff): This example demonstrates how to
  generate a Sierpinski triangle.
- [Sine Wave](sine.ff): This example demonstrates how to generate an animated
  sine wave.
- [Assembly Sum (aarm64)](sum_aarm64.ff): This example demonstrates how to write
  a function in assembly and call it from `frost`.
- [Echo Server](echo.ff): This example demonstrates how to write a server that
  echos to the client.

## Running the Examples

To run an example, you can use the `frost` compiler to compile the example file
to LLVM IR, and the use [`lli`](https://llvm.org/docs/CommandGuide/lli.html) to
run the LLVM IR. For example, to run the Mandelbrot set example, you can run the
following commands:

```sh
$ ./glados -i mandelbrot.ff -o mandelbrot.ll
```

> [!TIP]
> Use LLVM's [`opt`](https://llvm.org/docs/CommandGuide/opt.html) tool to
> optimize the generated LLVM IR before running it with `lli`.
>
> ```
> opt -passes=mem2reg -S mandelbrot.ll -o mandelbrot.opt.ll
> ```

```sh
$ lli mandelbrot.ll
```

When you run the above commands, the Mandelbrot set will be generated and
displayed in the terminal.

```
                                      @@@@@@                                   
                                      @@@@@@@                                  
                                        @@. .                                  
                           .@@  @@@@@@@@@@@@@@@@@@@   @                        
                           .@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
                        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
                       @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                    
       @    @         :@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                     
      .@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                     
    .@@@@@@@@@@@@@@@ @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@.                     
.@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
.@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
    .@@@@@@@@@@@@@@@ @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@.                     
      .@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                     
       @    @         :@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                     
                       @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                    
                        @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
                           .@@@@@@@@@@@@@@@@@@@@@@@@@@@@                       
                           .@@  @@@@@@@@@@@@@@@@@@@   @                        
                                        @@. .                                  
                                      @@@@@@@                                  
                                      @@@@@@
```

### Writing a TCP echo server in `frost`

1. Use the standard library to write your server

Have a look at [`echo.ff`](echo.ff) for the acutal implementation

2. Compile the program

```sh
$ ./glados -i echo.ff -o echo.ll
```

3. Run the server

```sh
(server) $ lli echo.ll
Listening on port 8001
Connection accepted
Message Received: Hello world!
```

4. Connect to the server using `nc` or `telnet`

```sh
(client) $ nc -v localhost 8001
Connection to 127.0.0.1 port 8001 [tcp/vcom-tunnel] succeeded!
Hello world!
Hello world!
```

### Embedding `frost` in other languages

1. Create a `sum.ff` file with the following content:

```
sum: int int -> int = a b {
  a + b
}
```

2. Compile the `sum.ff` file to LLVM IR:

```sh
$ ./glados -i sum.ff -o sum.ll
```

#### Embedding in `C`

Since `frost` compiles to LLVM IR, you can embed `frost` code in your C, C++ or
C-API compatible code. For example, to embed the `sum` function in a C program,
you can follow the steps below:

1. Compile the following C program that embeds the `sum` function:

```c
#include <stdio.h>

extern int sum(int a, int b);

int main()
{
    int a = 10;
    int b = 20;
    int result = sum(a, b);
    printf("The sum of %d and %d is %d\n", a, b, result);

    return 0;
}
```

> [!WARNING]
> Since `clang` uses LLVM as its backend, you can use `clang` to compile the
> generated LLVM IR directly. If you want to use `gcc` or another compiler, you
> must first convert the LLVM IR to an object file using `llc` and then compile
> the object file with `gcc`.
>
> ```sh
> $ llc -filetype=obj sum.ll -o sum.o
> $ gcc -o sum sum.o sum.c
> ```

2. Compile the C program and link it with the `sum.ll` file:

```sh
$ clang -o sum sum.c sum.ll
```

3. Run the compiled program:

```sh
$ ./sum
The sum of 10 and 20 is 30
```

#### Using `dlopen` and `dlsym`

You can also use `dlopen` and `dlsym` to load the `sum` function at runtime. To
do this:

1. Compile the `sum.ll` file to a shared object file:

```sh
$ llc -filetype=obj sum.ll -o sum.o
```

2. Create a shared object file from the object file:

```sh
$ clang -shared -o libsum.so sum.o
```

3. Use a language that supports `dlopen` and `dlsym` to load the `sum` function
   at runtime. For example, you can use [Deno](https://deno.com/) to write a
   TypeScript program that loads the `sum` function at runtime:

```ts
const handle = Deno.dlopen("libsum.so", {
  sum: {
    parameters: ["i32", "i32"],
    result: "i32",
  },
});

const sum = handle.symbols.sum as CallableFunction;

console.log(`The sum of 10 and 20 is ${sum(10, 20)}`);

handle.close();
```

4. Run the program:

```sh
$ deno run --allow-all sum.ts
The sum of 10 and 20 is 30
```
