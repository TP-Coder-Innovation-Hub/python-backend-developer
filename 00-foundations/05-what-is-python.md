# What is Python

`[Entry]`

Python is a high-level, general-purpose programming language created by Guido van Rossum and first released in 1991. It was designed with one overriding goal: make code readable.

## Design Philosophy

Python's design principles are documented in "The Zen of Python" (accessible by running `import this` in a Python shell). The key ideas:

- **"Beautiful is better than ugly."** Code is read far more often than it is written. Python optimizes for readability.
- **"There should be one -- and preferably only one -- obvious way to do it."** Python avoids giving you five different syntaxes for the same thing. One canonical approach.
- **"Explicit is better than implicit."** Behavior should be visible, not hidden behind magic.
- **"Simple is better than complex."** If a simple solution works, use it.

These aren't just slogans. They shape every decision in the language. When Python adds a feature, it has to justify itself against these principles.

## What Python Looks Like

```python
def greet(name):
    return f"Hello, {name}!"

message = greet("Ada")
print(message)
```

Compare the same logic in Java:

```java
public class Greeter {
    public static String greet(String name) {
        return "Hello, " + name + "!";
    }

    public static void main(String[] args) {
        String message = greet("Ada");
        System.out.println(message);
    }
}
```

Same result. Python uses 4 lines; Java uses 10. Python uses indentation for structure; Java uses braces. Python infers types at runtime; Java requires explicit type declarations.

This is not a judgment against Java -- Java's explicitness is valuable in large enterprise codebases. But for most tasks, Python gets you to a working solution with less code.

## Where Python Fits in 2026

| Domain | Python's Role |
|--------|--------------|
| Web backends | Dominant. FastAPI and Django power millions of services. |
| ML / AI | The default language. PyTorch, TensorFlow, LangChain are Python-first. |
| Data science | Standard. Pandas, NumPy, Jupyter are the ecosystem. |
| Scripting / automation | Go-to choice. sysadmin tools, CI/CD scripts, data pipelines. |
| Desktop apps | Possible (Tkinter, PyQt) but not Python's strength. |
| Mobile apps | Rare. Not Python's domain. |
| Systems programming | Rare. Use Go or Rust for this. |

## Python Versions

The Python ecosystem has standardized on Python 3.12+ as of 2026. Python 2 reached end-of-life in 2020 and is dead.

Key modern Python versions:

| Version | Year | Notable Feature |
|---------|------|----------------|
| 3.10 | 2021 | Pattern matching (`match/case`) |
| 3.11 | 2022 | Significant speed improvement, `TaskGroup` |
| 3.12 | 2023 | Better error messages, type parameter syntax |
| 3.13 | 2024 | Free-threaded mode (experimental, PEP 703) |

If you're starting a new project, use Python 3.12 or 3.13.

## The Takeaway

Python is a language that prioritizes developer productivity and code readability over raw execution speed. For backend development -- where most time is spent waiting on databases and network calls -- this is exactly the right tradeoff.
