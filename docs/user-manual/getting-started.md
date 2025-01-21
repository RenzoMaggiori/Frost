# Getting Started with Frost

## Quick Start: Using Pre-built Binaries

Getting started with Frost is as simple as downloading the latest release for
your platform. The pre-built binaries offer the fastest way to begin your
journey with Frost programming. Currently available for `Linux` (x86_64) and
`macOS` (x86_64 and arm64).

**Download and Install**

1. Navigate to the [releases page](https://github.com/RenzoMaggiori/Frost/releases/)
2. Download the latest version for your operating system
3. Extract the archive to your preferred location
4. Add the `frostc` binary to your system's PATH
5. Verify the installation:

```bash
frostc -h
```

## Building from Source

For those who prefer to build from source or need the latest development
version, follow these steps for a complete development environment setup.

**Prerequisites**

- Git version control system
- GHC 9.4.8 (Haskell compiler)
- Cabal package manager
- LLVM 19 toolchain
- Make build system

**Installing Dependencies**

First, ensure you have GHCup installed for managing Haskell tools:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

Install the required Haskell components:

```bash
ghcup install ghc 9.4.8
ghcup install cabal latest
ghcup set ghc 9.4.8
```

**Building Frost**

1. Clone the repository with submodules:

```bash
git clone --recursive https://github.com/RenzoMaggiori/Frost.git
cd frost
```

2. Build the compiler:

```bash
make
```

The build process will compile all dependencies and create the `frostc` binary
in the repository root.

**Verifying Installation**

Create a simple Frost program to verify your installation:

```frost
import "https://frost-lang.deno.dev/std/io.ff"

main: int -> int = argc {
    printf("Hello! This program has %d arguments\n" argc)

    0
}
```

Save it as `hello.ff` and use
[`lli`](https://llvm.org/docs/CommandGuide/lli.html) to JIT-compile and run the
program:

```bash
./frostc -i hello.ff -o hello.ll | lli
```

## Next Steps

With Frost installed, you're ready to start creating beautiful systems programs.
Continue to the next section to learn about Frost's core concepts and begin
writing your first real program.

Remember to check out the examples directory in the repository for inspiration
and common patterns in Frost programming.
