# Building Frost from Source

This guide will walk you through the process of building the Frost compiler from
source. Building from source allows you to work with the latest development
version and contribute to the project.

## Prerequisites

Before you begin, ensure you have the following tools installed:

1. **GHC 9.4.8**: The Glasgow Haskell Compiler version 9.4.8
2. **Cabal**: The latest version of the Cabal build system
3. **Git**: For cloning the repository and managing submodules
4. **LLVM 19+**: The LLVM compiler infrastructure, version 19 or newer
5. **Make**: The GNU Make build automation tool

## Step-by-Step Instructions

1. **Clone the Repository**

   Clone the Frost repository and initialize its submodules:

   ```bash
   git clone --recursive https://github.com/RenzoMaggiori/Frost.git
   cd frost
   ```

2. **Set Up GHC and Cabal**

   Ensure you're using GHC 9.4.8. If you have GHCup installed, you can run:

   ```bash
   ghcup install ghc 9.4.8
   ghcup set ghc 9.4.8
   ```

3. **Install Dependencies**

   Install the project dependencies using Cabal:

   ```bash
   cabal update
   cabal install --only-dependencies
   ```

4. **Build the Compiler**

   From the root directory of the project, run:

   ```bash
   make
   ```

   This command will compile the Frost compiler and generate the `frostc`
   executable in the project root.

5. **Verify the Installation**

   To ensure the compiler was built successfully, run:

   ```bash
   ./frostc -h
   ```

   This should display the version information for the Frost compiler.

## Documentation

We maintain Haddock documentation for the Frost compiler. It is currently hosted
on GitHub Pages and can be accessed at
[https://jabolol.github.io/glados/](https://jabolol.github.io/glados/).

To generate the documentation locally, run the following command:

```bash
cabal haddock
```

## Troubleshooting

- If you encounter LLVM-related errors, make sure you have LLVM 19 or newer
  installed and that it's in your system PATH.
- For any Haskell package dependency issues, try running `cabal update` before
  attempting the build process again.

## Contributing

After successfully building Frost from source, you're ready to start
contributing to the project. Be sure to read our contribution guidelines and
coding standards before submitting any pull requests.

Remember to keep your local copy of the repository up to date by regularly
pulling changes and updating submodules:

```bash
git pull
git submodule update --init --recursive
```

By building Frost from source, you're not only getting the latest features but
also positioning yourself to contribute to the language's development. Happy
coding!

### Pre-Commit Hooks

This repository uses pre-commit hooks to ensure code quality. To install the
hooks, run the following command:

```bash
pre-commit install
```

Hooks for code formatting and linting will run automatically when you commit
changes. These hooks are `ormolu` and `hlint`. Please make sure to fix any
errors before committing. You can also run the hooks manually using:

```bash
pre-commit run --all-files
```

### Devcontainer

This project includes a `devcontainer` configuration for Visual Studio Code. To
use the devcontainer, you need to have
[Docker](https://docs.docker.com/get-docker/),
[Visual Studio Code](https://code.visualstudio.com/) and the
[Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
extension installed.

To open the project in the devcontainer, open the command palette
(`Ctrl+Shift+P`) and run the command `Remote-Containers: Reopen in Container`.
All the necessary tools and dependencies will be installed in the container.

### Development Workflow

When working on the Frost compiler, you can use the following commands to build
the project:

```bash
cabal build
```

To run the project, run the following command, replacing `examples/hello.ff`
with the path to your Frost source file:

```bash
cabal run frostc -- -i examples/hello.ff
```
