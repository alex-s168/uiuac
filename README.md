# Uiua Compiler
[Uiua](https://uiua.org) native compiler using MLIR and LLVM.

uiuac can also be used to generate a graph of your code.

uiuac development gets discussed on the [Uiua Discord server](https://discord.gg/bs9RySbE8R), in [this thread](https://discord.com/channels/1156339038748413952/1247846178645872661).

## Advantages
- no interpreter overhead (variables, op execution, error & type checking)
- less memory and memory allocations required (because smaller element types in arrays can be used and array allocations are optimized away in most cases)
- **vectorization across multiple operations**
- no need for optimizing specific code patterns: almost all ways of implementing code run at same speed
- more cache and memory efficient array shapes can be chosen
- code can be compiled for your current target, allowing for usage of processor extensions not present in every processor (for example AVX-512)
- Uiua code could theoretically run well on microprocessors
- more opertunities for high-level optimizations
- uiuac will be able to detect parallelizable loops that take a lot of time to execute and make them execute in multiple threads
- not limited to CPU: in the future, uiuac will be able to execute code on GPUs and maybe even TT AI accelerators (which have interesting advantages over GPU)
- absolutely no overhead for calls to native functions (FFI)
- ability to use code written Uiua as native functions which can be used in almost every programming language (C, Rust, ...)

The main goal of uiuac is to maximize both scalar and vector execution speed to a point where it is comparable to optimized code written in low-level programming languahes

## Compilation Process
- (Uiua interpreter) generation of `.uasm` file
- (uiuac) conversion from `.uasm` to uiuac IR (`.uac`)
- (uiuac) lowering of Uiua operations to simple loops, loads, stores, ...
- (uiuac) high- and low- level optimizations
- (uiuac) emit MLIR
- (mlir) high- and low- level optimizations
- (mlir) emit LLVM IR
- (llvm) low-level optimizations, vectorization, compile to binary

## Contributing
uiuac is still missing support for a lot of Uiua operations

## Using 
this project is still very WIP, but you can already compile simple uiua programs.

To do that, you need Clang, MLIR, and a JDK installed.
You need the following things in your path: `clang`, `mlir-opt`, `mlir-translate`.

You need to run this once if you want to build executables
```shell
# compile uiuac runtime
cd rt && ./build.sh && cd ..
```

Now you are ready to compile uiua programs.
If you have bash installed, run:
```shell
./uiuac --pipeline=uiua-exe --in=MyUiuaFile.ua --out=out.exe
```

If you don't have bash installed, run (not recommended):
```shell
./gradlew run --args "--pipeline=uiua-exe --in=MyUiuaFile.ua --out=out.exe"
```

for more features (like generating graphs), see the help text (`./uiuac --help` or `./gradlew run --args "--help"`)
