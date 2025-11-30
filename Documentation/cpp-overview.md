# C++ Implementation Overview in SplashKit Online

## Introduction

This document gives overview of how in Splashkit Online, code is  compiled and execuded. It describes how user-written C++ code flows from the browser editor to WebAssembly compilation and execution.

Main purpse is to help new contributors understand that how the system integrates C++ capability without requiring in-depth compiler knowledge.

## Where C++ Lives in Repository

- **compilers/cxx:**  
Includes the primary logic for applying Clang to compile C++ code to WebAssembly.
- **runtimes/cxx:**
Manages the browser's runtime execution of C++ programmes that have been compiled.
- **SplashKitWasm:**
Contains the WebAssembly build of the SplashKit library used by compiled programs.
- **javascript/compilers/cxxCompiler.js:**
Manages the front-end compilation process.

## Role of CXXCompiler

CXXCompiler class is responsible for managing the C++ complition process. Its responsibilities include:

- Writing source files to virtual filesystem
- Compile each file of C++ into object files
- Link object files into final WebAssembly output
- Provide syntax checking and error reporting
- Warn users about unsupported API usage

It communicates with Web Worker to make sure that main interface doesn't freeze during compilation. 


## Web Workers and Clang Backend

In a Web Worker, the actual compilation process is carried out using:

- **clang++.wasm** Compiles C++ source into object files
- **wasm-ld.wasm**  Links object files into a final WASM executable

In order to maintain UI responsiveness, this occurs asynchronously.

## Relationship with WebAssembly (WASM)

SplashKit Online can securely run C++ code within the browser thanks to WebAssembly.
The browser's runtime interprets WASM, which is created by the compiler from C++.
This method offers:

- Sandboxing for security
- Compatibility between platforms
- Execution in real time in a web setting

## Execution Environment

Runtime environment handles:

- Program start / stop / pause
- File system interactions
- Canvas rendering
- Audio output
- Keyboard and user input

The ExecutionEnvironmentInternalCXX class is in charge of this, coordinating communication between the browser user interface and the compiled programme.

## Error Handling and Output

Prior to being displayed to the user, compiler messages undergo processing and formatting. In order to emphasise certain lines and offer insightful terminal feedback, errors are analysed.
