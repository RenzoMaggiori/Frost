# Frost

**Frost** is a modern systems programming language that combines the speed of
`C` with modern language features. It offers static typing and compiles to
[LLVM IR](https://llvm.org/docs/LangRef.html), delivering high performance while
remaining accessible and easy to learn. Whether you're building system tools or
performance-critical applications, Frost provides the low-level control you need
with the convenience of contemporary programming practices.

## Example

> [!TIP]
> For examples and sample code, check the [examples](examples) directory. These
> demonstrate the key features of the language, including basic syntax, data
> structures, and how to interact with the system.

Here's a simple "Hello, world!" program written in Frost:

```frost
import "https://frost-lang.deno.dev/std/io.ff"

main: never -> int = {
  printf("Hello, world!\n")

  0
}
```

To compile and run the program, save it to a file (e.g., `hello.ff`) and use the
Frost compiler:

```
$ frostc -i hello.ff | lli
Hello, world!
```

## Features

- **Static Typing**: Catch type errors at compile time, improving safety and
  efficiency.
- **LLVM Backend**: Generates optimized code, leveraging the powerful LLVM
  ecosystem.
- **Low-Level Control**: Access hardware resources and memory directly, similar
  to C/C++.
- **Modern Syntax**: Designed to be more readable and maintainable compared to
  traditional low-level languages.

## Getting Started

To begin with Frost, ensure that [Haskell](https://www.haskell.org/) and
[LLVM 19](https://llvm.org/) or later are installed on your system. You can
download the latest release from the [releases page](README.md) or opt to build
Frost from source. For comprehensive installation guidance, please consult the
[Documentation](https://frost-lang.gitbook.io/frost/user-manual/getting-started).

## License

Frost is licensed under the MIT License. See [LICENSE](LICENSE) for details.
