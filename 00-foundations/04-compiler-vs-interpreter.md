# Compiler vs Interpreter

`[Entry]`

Code is written for humans. Computers run machine code (1s and 0s). Something has to translate between the two. There are two main strategies: compiling and interpreting.

## Compiled Languages

A compiler translates your entire program into machine code *before* it runs. You get a standalone executable file.

```
Source code  -->  [Compiler]  -->  Machine code (executable)  -->  Run
```

The compiler reads all your code, checks for errors, optimizes performance, and produces a binary. You distribute the binary, not the source code.

| Compiled Language | Characteristics |
|-------------------|----------------|
| C | Fast, manual memory management, no runtime overhead |
| C++ | Fast, complex, used for games and systems |
| Go | Fast compilation, built-in concurrency, simple syntax |
| Rust | Fast, memory-safe without garbage collection |

**Pros:** Fast execution (optimized ahead of time), catches errors before running, single binary distribution.

**Cons:** Slower development cycle (compile after every change), platform-specific binaries (a Linux binary won't run on Windows).

## Interpreted Languages

An interpreter reads and executes your code *line by line at runtime*. No separate compilation step.

```
Source code  -->  [Interpreter reads and executes line by line]
```

| Interpreted Language | Characteristics |
|---------------------|----------------|
| Python | Easy to read, huge ecosystem, slower execution |
| Ruby | Developer-friendly, web-focused (Rails) |
| JavaScript | Runs in browsers and servers (Node.js) |

**Pros:** Fast development cycle (run immediately after editing), platform-independent (same code runs everywhere), interactive (try code in a REPL).

**Cons:** Slower execution (translating on the fly), errors surface at runtime (not before), needs the interpreter installed to run.

## Python's Approach (The Nuance)

Python is called "interpreted," but it's more nuanced than that. When you run a Python file:

1. Python **compiles** your source code to bytecode (a lower-level representation).
2. Python's **virtual machine** (the interpreter) executes that bytecode line by line.

```
Source (.py)  -->  Bytecode (.pyc)  -->  Python VM executes bytecode
```

The bytecode is cached in `__pycache__/` directories. If you haven't changed the source file, Python reuses the cached bytecode instead of recompiling. This is why Python feels interpreted (no manual compile step) but isn't purely interpreted either.

## What This Means for You

| Concern | Compiled (Go, Rust) | Interpreted (Python) |
|---------|---------------------|---------------------|
| Development speed | Edit, compile, run | Edit, run |
| Execution speed | Fast | Slower |
| Error detection | Before running | At runtime |
| Distribution | Single binary | Needs Python installed |
| Interactive testing | No | Yes (REPL) |

For backend development, execution speed rarely matters as much as development speed. Your API spends most of its time waiting on databases and network calls, not crunching CPU cycles. Python's interpreted nature means you can iterate quickly, test ideas fast, and debug interactively.

The tradeoff is worth it for most backend services.
